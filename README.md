# Collaborator Everywhere (with IP support)

**This is a fork of [@albinowax's](https://github.com/albinowax) Collaborator Everywhere plugin which works with IP addresses (instead of domain names). It can be useful when testing the application inside internal networks that have no access to and cannot be accessed from the internet.**

For information about Collaborator Everywhere, please refer to the [original repository](https://github.com/PortSwigger/collaborator-everywhere/).

## What has changed

1. It checks whether Burp Collaborator Server location is in domain format (like oastify.com/yu8990wrjiubqd253...) or IP address one (like 10.16.0.1).
2. For domain format, the *injection headers* are as in original. For IP address format the payloads are in a [separate file](https://github.com/afine-com/collaborator-everywhere-with-ip-support/blob/master/resources/injections-ip-address-mode).
Bear in mind that in many cases pingbacks were made by DNS, and it is not possible to achieve that using only IP addresses.
   
# Usage
## Start Collaborator Server
###Prepare configuration file for Burp Collaborator:
Replace `192.168.1.55` with your IP address. 
```
{
    "workerThreads":10,
    "eventCapture": {
        "localAddress":["192.168.1.55", "127.0.0.1"],
        "publicAddress":"192.168.1.55", "http": { "ports": 80 },
        "polling": {
            "localAddress": "127.0.0.1",
            "publicAddress": "192.168.1.55",
            "http": { "ports": 8080 },
        }
    },
    "logLevel": "DEBUG"
}
```
Save it as *local-collaborator.cfg*

### Run collaborator server
You have to use Burp Suite JAR. Remember to change the paths. 
```
sudo java -jar burpsuite_pro.jar --collaborator-server --collaborator-config=local-collaborator.cfg
```

## Configure Burp Collaborator Server
1. Project options > Burp Collaborator Server > Use a private Collaborator server (check).
2. Set *Server location* for your local IP address.
3. Select *Poll over unencrypted HTTP*.
4. Click `Run health check ...` - everything apart from HTTPS, SMTPS and DNS should show *Success*.

## Install Collaborator Everywhere (with IP support)
1. Download the newest JAR from [Releases](https://github.com/afine-com/collaborator-everywhere-with-ip-support/releases/).
2. Extender > Extensions > Add
3. Click `Select file ...` in *Extension file (.jar)* and choose downloaded JAR file.

You should see following output:
```
Collaborator IP address mode (payloads will be made using the IP address instead of domain)
Restart the plugin if you change Collaborator Server Mode.
Calculated your IPs: [192.168.1.55]
Loaded Collaborator Everywhere (with IP support) v1.3
```

Observe in *Logger* that headers are added to the requests in *Scope*.

**IMPORTANT: If you change Burp Collaborator Server settings, restart the extension!** 

### Fork credits
[@linoskoczek](https://github.com/linoskoczek) & [@mackeysec](https://github.com/mackeysec) (AFINE Team)