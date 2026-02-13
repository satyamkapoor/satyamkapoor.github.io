---
title: "Setting up cloud storage service using Nextcloud & Docker"
date: 2020-12-13T19:38:49+01:00
toc: false
images:
tags:
  - raspberrypi
  - selfhosted
categories:
- tech
summary: Hosting a Nextcloud instance using Docker on raspberry pi @ home
description: Setting up your private cloud storage service using Nextcloud & Docker
---

#### Motivation

- **Cut down fixed monthly expense**: Google recently [announced](https://support.google.com/photos/answer/6220791) that High Quality pictures stored on Google Photos will start counting towards the free Google Account storage (15 GB) starting from June 2021. I'm a great fan of Google Photos & have promoted it a lot back in the days when it was still a new thing but I've been opposing the subscription based model to cut down on my fixed monthly expenses.

- **Increased sense of privacy**: All of our data resides with half a dozen of companies who's name change per country (Google, vk, wechat, Tencent, Microsoft). They know a lot about us :) and power might get abused.

- **Cool to have your own domain associated with your cloud**: At times it's good for an individual/SME to have their cloud sharing links associated with their domain. (<s>https://drive.google.com/file/d/toolongabsurdlink</s> -> https://cloud.yourname.com/share/longbutyourprimarydomainlink)

#### What is Nextcloud?

[Nextcloud](https://nextcloud.com/) is an open source application which you can host on any machine to make your own private cloud with your control. It allows domain association. So your file storage can be accessed using `cloud.yourname.com`.


#### Where to host?
You can host Nextcloud on any linux machine (or perhaps on Windows as well). I'll strongly suggest to take this decision based on

- Your storage needs &
- Data transfer limits (if any)

[Hetzner](https://www.hetzner.com/), [DigitalOcean](https://www.digitalocean.com/) & [Linode](https://linode.com/) are some cheap and quality hosting providers which I'll suggest to go with.

Since I had a 1TB Hard disk along with a raspberry pi 3b+ lying around, I decided to save on some money and use these stuff.

#### How to host?

I decided to use mariadb server along with Nextcloud. So I started two docker containers with mariadb-server and Nextcloud images respectively. Here's the docker-compose file you can use to set Nextcloud & Mariadb. I went ahead using mounted 1TB HDD with Nextcloud container.


``` yml
version: '3.9'
services:
  nextcloud:
    image: nextcloud:apache
    restart: always
    ports:
      - 8080:80
    volumes:
      - /mnt/hdd/nextcloud_data:/var/www/html
    networks:
      - nextcloud-network
    environment:
      - MYSQL_HOST=db
      - MYSQL_DATABASE=nextcloud
      - MYSQL_USER=nextcloud
      - MYSQL_PASSWORD=nextcloud
  db:
    image: mariadb
    command: --transaction-isolation=READ-COMMITTED --binlog-format=ROW
    restart: always
    volumes:
      - /mnt/hdd/mariadb_data:/var/lib/mysql
    networks:
      - nextcloud-network
    environment:
      - MYSQL_DATABASE=nextcloud
      - MYSQL_USER=nextcloud
      - MYSQL_ROOT_PASSWORD=nextcloud
      - MYSQL_PASSWORD=nextcloud
      - NEXTCLOUD_TRUSTED_DOMAINS=cloud.mydomain.com playground.mydomain.com 192.168.0.65

networks:
  nextcloud-network:
```

I had apache2 installed on my pi running pi-hole. So I decided to set a reverse proxy going to the Nextcloud container which was being exposed on port 8080. Here's the **Apache's VHost Configuration**. Note I make sure the `/.well-known/acme-challenge` path is not proxied. This comes in handy to use certbot for SSL certificate generation from [Let's Encrypt](https://letsencrypt.org/). 

``` xml
<VirtualHost *:80>
    ServerName cloud.abc.com
    ProxyPass /.well-known/acme-challenge ! #For DNS verification by Let's Encrypt
    ProxyPass / http://localhost:8080/
    ProxyPassReverse / http://localhost:8080/


RewriteEngine on
RewriteCond %{SERVER_NAME} =cloud.abc.com
RewriteRule ^ https://%{SERVER_NAME}%{REQUEST_URI} [END,NE,R=permanent]
</VirtualHost>
```


The very first time when you'll try opening Nextcloud, it will prompt you to set your credentials and configuration for the database server instance.
Once done, your personal cloud should be ready.



![Example image](/nextcloud_login.jpg)

#### Apps to install on Nextcloud hub
Nextcloud hub is filled with great apps. Some of the apps which I'll personally recommend are:

- Contacts
- Calendar
- Phonetrack
- [Notes](https://apps.nextcloud.com/apps/notes)


#### Applications for Android


**[DAVx5](https://apps.nextcloud.com/apps/davx5_caldav_carddav_sync_suite_on_android)** - Syncs contacts & calendars bidirectionally from my phone & pi


**[Nextcloud](https://play.google.com/store/apps/details?id=com.nextcloud.client&hl=en&gl=US)** - The official nextcloud app. You'll have to enter your credentials and things are good to go. Provides functionality to auto upload images (similar to Google Photos - Auto Backup).


**[Phonetrack](https://f-droid.org/de/packages/net.eneiluj.nextcloud.phonetrack/)** - _Find my Phone_ like functionality with just Google not storing or knowing where your phone is.

**[Notes](https://play.google.com/store/apps/details?id=it.niedermann.owncloud.notes&hl=en&gl=US)** - A _Google Keep_ alternative. A note-taking app where all your notes are stored on your nextcloud server as .txt

#### Downsides of nextcloud or the whole process

- Internet speed: I have a DSL at home with slow speed which means whenever I'm trying to access a file, it is trying to upload it at the rate of _9.75 Mbit/s_.
```
Retrieving speedtest.net configuration...
Testing from ISP (37.112.11.121)...
Retrieving speedtest.net server list...
Selecting best server based on ping...
Hosted by Proximus [7.34 km]: 14.73 ms
Testing download speed.................................................
Download: 92.74 Mbit/s
Testing upload speed...................................................
Upload: 9.75 Mbit/s
```

To solve this partially, I added the local IP of my raspberry pi in the local DNS (alternatively you can add this in your `/etc/hosts` file). This makes sure that when I'm accessing nextcloud from within my network, the speed is faster (my network speed at play rather than Internet's).

To streamline this, we can utilise caching, or move to a better internet or host it with any of the IaaS cloud providers.

- Conflicts while auto-uploading images

This error keeps appearing back. The impact isn't much because it does upload all files but the notification is annoying. I'll update you once I've figured out the root cause here. 

![Nextcloud issues](/nextcloud_sync.jpg)

#### Overall?

Nextcloud is free, open-source & has a huge community of developers. My current issues are mostly related to my efforts to save costs. If I'd be hosting this on Linode or DigitalOcean the experience should be smoother.

I'm still happy that my data is in my control and I'm able to associate my domain with my cloud storage.

#### Next steps

- Setup [caching](https://docs.nextcloud.com/server/15/admin_manual/configuration_server/caching_configuration.html): APCu, Redis or Memcached. APCu should be best for my setup as it's single node.

- Buy another 1TB HDD and setup RAID1 using [mdadm](https://linux.die.net/man/8/mdadm).

- Setup daily backups to an archival service like S3 glacier.

- Setup monitoring and logging.



Please feel free to contact me via [Twitter](https://twitter.com/imsatyamkapoor) for suggestions or improvements.
