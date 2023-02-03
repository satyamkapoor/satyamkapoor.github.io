---
date: "2020-09-24"
description: A python script to regularly update DNS with your unstable home IP
slug: ddns raspberry pi 
tags:
- raspberrypi
- DNS
categories:
- tech
title:  DDNS for raspberry pi
summary: A python script to regularly update DNS with your unstable home IP
---


#### Problem Statement

One main issue setting up a small web server or VPN server at home is managing your public IP. Most home ISPs do charge extra for issuing you a static public IP address. If you do not want to pay then you need to tackle this problem yourself. 

#### Solution
You can setup a subdomain/domain with your home public IP address (say an A record for home.example.com or pi.example.com which points to your public IP). Then run a script which periodically checks if your home public IP has been changed & updates the DNS accordingly.


Here, I have written a tiny script which checks if my current home IP has changed. If positive, it updates the DNS *A record* associated with my home domain.

I manage my DNS with digitalocean (www.digitalocean.com = DO). DO provides a REST API which can be used to update your configuration.

Assuming I have created a A record with name mypi.satyamkapoor.com with a value of myPublicIP in DO DNS. I'll first have to get the `id` of the domain record. You can do this by 

```
curl -X GET -H "Content-Type: application/json" -H "Authorization: Bearer b7d03a6947b217efb6f3ec3bd3504582" "https://api.digitalocean.com/v2/domains/example.com/records"
```
Once you have your domain record id, you can set up the following Python script as a cron which will update your DNS A record whenever your home IP has been changed.


``` python
import subprocess, requests, logging, shlex

API_SECRET = "YOUR_DIGITAL_OCEAN_API_KEY" 
DOMAIN_RECORD = "yourdomainrecordID"
DOMAIN_NAME = "example.com"

def getCurrentIP():
  cmd='dig @resolver1.opendns.com ANY -4 myip.opendns.com +short'
  proc=subprocess.Popen(shlex.split(cmd),stdout=subprocess.PIPE)
  out,err=proc.communicate()
  currentIPv4 = out.decode('UTF-8').strip('\n')
  return currentIPv4

def validateAndUpdateIPLocally(currentIP):
        with open('current_ip.txt','r') as inputfile:
                lines = inputfile.readlines()
                if lines:
                        lineCounter = 0
                        for line in lines:
                                lineCounter = lineCounter + 1
                                if lineCounter < 2:
                                        if currentIP == line:
                                                break
                                        else:
                                                fileUpdate = open("current_ip.txt", 'w')
                                                fileUpdate.write(currentIP)
                                                url = "https://api.digitalocean.com/v2/domains/"+ DOMAIN_NAME + "/records/" + DOMAIN_RECORD
                                                headers = {'Content-Type':'application/json','Authorization': 'Bearer ' + API_SECRET}
                                                payload = {"data": currentIP}
                                                r = requests.put(url, headers=headers, json = payload)
                                                logging.basicConfig(filename='HomeIPV4.log',level=logging.DEBUG,format='%(asctime)s %(message)s', datefmt='%m/%d/%Y %I:%M:%S %p')
                                                logging.info('Text:'+r.text+" status code "+ str(r.status_code) )


currentIP = getCurrentIP()
validateAndUpdateIPLocally(currentIP)
```

This script will use two files namely `current_ip.txt` and `HomeIPV4.log` for storing the current live public IP and logs of the script respectively.

I'll try to extend this script for AWS Route53 soon.

Thanks for reading please feel free to message me in case of suggestions or queries.