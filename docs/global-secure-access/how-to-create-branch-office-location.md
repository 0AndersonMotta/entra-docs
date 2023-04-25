---
title: How to create branch office locations
description: Learn how to create branch office locations for Global Secure Access.
author: kenwith
ms.author: kenwith
manager: amycolannino
ms.topic: how-to
ms.date: 04/25/2023
ms.service: network-access
ms.custom: 
---

# Learn how to create a branch office locations for Global Secure Access

Learn how to create a branch office location for Global Secure Access.

## Pre-requisites 
- Microsoft Entra Internet Access premium license for your Microsoft Entra Identity tenant.  
- Entra Network Access Administrator role in Microsoft Entra Identity.
- The *Microsoft Graph* module must be installed to use PowerShell.
- Admin consent is required when using Graph explorer for the Microsoft Graph API. 

## Create a branch location
Global Secure Access provides branch connectivity so you can connect a branch office to the Microsoft network. Once a branch is connected to the Microsoft network, you can set up network security policies. These policies are applied on all outbound traffic. Alternatively, you can set up clients on individual devices to connect to the Microsoft network regardless of the device location and Internet connection. To learn more about the client for Global Secure Access, see [How to install the Windows client](how-to-install-windows-client.md).

There are multiple ways to connect a branch location to the Microsoft network. In a nutshell, you are creating an IPSec tunnel between a core router at your branch location and the nearest Microsoft VPN service. The network traffic is routed with the core router at the branch location and thus installation of a client is not required on individual devices.

### Create a branch location using the Microsoft Entra admin center

1. Navigate to the Microsoft Entra admin center at `https://entra.microsoft.com` and login with administrator credentials.
1. In the left hand navigation choose **Global Secure Access**.
1. Select **Connect**.
1. Select **Branch**.
1. Select the link to open a form. The form is used to onboard your Microsoft Entra Identity tenant. You can also find the form at https://aka.ms/ztnaonboard.
1. Fill out the form with the required information and then select **Submit**.
    You cannot continue to the next step until your tenant has completed the onboard process. It takes up to 7 days for your tenant to go through the onboard process. 
1. Select **Create branch office** and enter:
    - Name: `ContosoBranch` 
    - Region: `EastUS` 
1. Select **Add link**. 
1. Under the **General** tab, enter the details: 
    - Link name: `CPE link 1` 
    - Device type: `Other` 
    - IP address: `<public IP address of your device>` 
    - Local BGP address: `<Specify the address of the local end of a BGP session>` 
    - Peer BGP address: `<Specify the address of the peer end of a BGP session>` 
    - Link ASN: `<specify the ASN` 
    - Redundancy: `No redundancy` 
    - Per tunnel bandwidth (Mbps): `500 Mbps` 
1. Select **Next**.
1. Under the **Details** tab, enter the details: 
    - Protocol: `IKEv2` 
    - IPSec/IKE policy: `Default` 

    Alternatively, you can select **IPSec/IKE** policy = `Custom` in above step. In this case, enter the details:
    - IKE Phase 1 
    - Encryption: `GCMAES128` 
    - IKEv2 integrity: `GCMAES128` 
    - DH Group: `14` 
    - IKE Phase 2 
    - IPSec encryption: `GCMAES128` 
    - IPSec integrity: `GCMAES128` 
    - PFS group: `PFS1` 
    - SA lifetime (seconds): `300` 
1. Select **Next**.
1. Under the **Security** tab, enter the details: 
    - Pre-shared key (PSK): `<Enter the secret key. The same secret key must be used on your CPE.>` 
1. Select **Add link**. 
1. Select **Next: Forwarding profiles**.
1. Select **M365 traffic profile: All M365 traffic**. 
1. Select **Review + Create**.


### Create a branch location using the Microsoft Graph API
1. Sign in to the Graph Explorer. 
1. Select POST as the HTTP method from the dropdown. 
1. Select the API version to beta. 
1. Add the following query to use Create Branches API (add hyperlink to the Graph API) 
    ```
    POST https://graph.microsoft.com/beta/networkaccess/branches 
    { 
        "name": "ContosoBranch", 
        "country": "United States ", //must be removed 
        "region": "East US", 
        "bandwidthCapacity": 1000, //must be removed. This goes under deviceLink. 
        "deviceLinks": [ 
        { 
            "name": "CPE Link 1", 
            "ipAddress": "20.125.118.219", 
            "version": "1.0.0", 
            "deviceVendor": "Other", 
            "bgpConfiguration": { 
                "ipAddress": "172.16.11.5", 
                "asn": 8888 
              }, 
              "tunnelConfiguration": { 
                  "@odata.type": "#microsoft.graph.networkaccess.tunnelConfigurationIKEv2Default", 
                  "preSharedKey": "Detective5OutgrowDiligence" 
              } 
        }] 
    }  
    ```
1. Select **Run query** to create a branch.

## Next steps
<!-- Add a context sentence for the following links -->
- [Update branch location](how-to-edit-branch-office-location.md)
