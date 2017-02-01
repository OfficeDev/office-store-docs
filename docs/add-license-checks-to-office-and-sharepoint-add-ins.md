---
title: Add license checks to Office and SharePoint Add-ins
ms.prod: MULTIPLEPRODUCTS
ms.assetid: eec76f9d-134a-4e88-b0c8-3d3067da2f61
ms.locale: en-US
---


# Add license checks to Office and SharePoint Add-ins
Add code to your Office and SharePoint Add-ins that checks the validity of a user's license, and takes action based on the license properties. Load test add-in license tokens to test your license checking code.
 

You can create and load test your Office Add-in licenses. To help you test your add-in's license-checking code, you can use test licenses. The Office runtime treats these test tokens as if they were valid tokens acquired from the Office Store, with the exception that tokens loaded through the registry are not tested for expiration or entitlement type. These test licenses are strings that conform to the  [Office and SharePoint Add-in license XML schema structure](office-and-sharepoint-add-in-license-xml-schema-structure.md).
 

To create a test token: 
