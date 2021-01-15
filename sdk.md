# Scratch Notes for the CTERA SDK for Python 

## Table of contents

1. [Set Up](#setup)
2. [Network Info](#netinfo)
3. [Cloud Services](#services)

### Set Up, Login, Insantiate a Filer <a name='setup'></a>

    >>> from cterasdk import *
    >>> admin = GlobalAdmin('<portal-url-or-ip')
    >>> admin.login('admin','<your-password>')
    2021-01-15 14:55:32,911    INFO [login.py:19] [login] - User logged in. {'host': 'todd.ctera.me', 'user': 'admin'}
    >>> filer = admin.devices.device('todd-vgateway')
    >>>

### Get Network Info <a name='netinfo'></a>

    >>> net_status = filer.network.get_status()
    >>> print(net_status)
    {
         "_classname": "PortStatus",
         "_uuid": "b1a59af3-ae1f-4a3f-a5c1-deadbdb47d57",
         "name": "LAN",
         "ip": {
              "address": "192.168.22.241",
              "netmask": "255.255.254.0",
              "gateway": "192.168.22.1",
              "DNSServer1": "192.168.22.12",
              "DNSServer2": "192.168.22.1",
              "DNSRotate": false,
              "connected": 1,
              "connectDate": 17,
              "leaseExpireDate": "2021-01-16T13:32:34"
         },
         "ethernet": {
              "on": true,
              "mac": "00:0c:29:18:f3:f7",
              "speed": 10000,
              "duplex": "Full"
         }
    }
    >>>

### Get Cloud Services info <a name='services'></a>

    >>> services = filer.services.get_status()
    >>> print(services)
    {
         "connected": true,
         "ipaddr": "192.168.22.173",
         "user": "Tenant Admin",
         "server_version": "6.1.1170.8",
         "server_address": "192.168.22.173",
         "last_connected_at": "2021-01-07T14:43:44"
    }
    >>>
