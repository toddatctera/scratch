# Scratch Notes for the CTERA SDK for Python 

## Table of contents

1. [Set Up](#setup)
2. [Cloud Sync](#sync)
2. [Network Info](#netinfo)
3. [Cloud Services](#services)

### Set Up, Login, Insantiate a Filer <a name='setup'></a>

    >>> from cterasdk import *
    >>> admin = GlobalAdmin('<portal-url-or-ip')
    >>> admin.login('admin','<your-password>')
    2021-01-15 14:55:32,911    INFO [login.py:19] [login] - User logged in. {'host': 'todd.ctera.me', 'user': 'admin'}
    >>> filer = admin.devices.device('todd-vgateway')
    >>>

### Get Cloud Sync status <a name='sync'></a>

    >>> sync = filer.sync.get_status()
    >>> print(sync)
    {
         "_classname": "CloudSyncConfStatus",
         "lastFailedScanTime": null,
         "selfVerificationScanningStubs": null,
         "id": "Synced",
         "uploadingFiles": 0,
         "filesFailedToUpload": 0,
         "filesFailedToUploadReal": 0,
         "uploadingFileName": null,
         "downloadingFiles": 0,
         "downloadingFileName": null,
         "downloadingMetadata": 0,
         "streamingFiles": 0,
         "onDemandFiles": 0,
         "metadataSize": 0,
         "lastSuccessfulScanTime": "2021-01-13T13:32:27",
         "scanningFiles": 0,
         "scanningStubs": 0,
         "removingDeletedFiles": 0,
         "filesCheckedForDelete": 0,
         "filesCheckedForDeletePercent": 0,
         "scanningFolderID": 0,
         "selfVerificationScanningFiles": 0,
         "selfVerificationRemovingDeletedFiles": 0,
         "selfVerificationFilesCheckedForDelete": 0,
         "selfVerificationFilesCheckedForDeletePercent": 0,
         "selfVerificationFolderID": 0,
         "totalFiles": 0,
         "totalSize": 0
    }
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
