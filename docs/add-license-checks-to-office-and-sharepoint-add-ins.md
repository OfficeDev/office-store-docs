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
 

- Copy the example in  [Office and SharePoint Add-in license XML schema structure](office-and-sharepoint-add-in-license-xml-schema-structure.md) license schema into a text file and save it with a .tok extension.
    
 
- Change the appropriate attributes, such as Product ID.
    
 
- Make sure the test attribute is present and set equal to true.
    
 
The Office Store verification web service, which you use to verify add-in license tokens, does not validate the encryption token or any of the attribute values of license tokens where the test attribute is set to  **true**. You can edit your test tokens directly and use them to test add-in behavior code based on different attribute values.
 
For Word, Excel, and PowerPoint add-ins: 
 

- Create your test tokens.
    
 
- Upload your test license tokens by using the developer registry. For details, see "Load a test license" later in this article.
    
 
For Outlook add-ins:
 

- Create your test token.
    
 
- Create a URL-encoded version of the add-in license token.
    
 
- In the add-in manifest file, manually edit the appropriate  [SourceLocation](http://dev.office.com/reference/add-ins/manifest/sourcelocation) element. Add the URL-encoded version of the license token to the source location URL as a query parameter named *et*  .
    
     **Note**  If your add-in uses  [getUserIdentityTokenAsync](http://dev.office.com/reference/add-ins/outlook/Office.context.mailbox), adding to the  [SourceLocation](http://dev.office.com/reference/add-ins/manifest/sourcelocation) element in the manifest will change the URL in the token because the token generation is based on what is in the manifest. When you test the license token, you will have to modify the validation call on your service so that the validation will accept the modified URL. For example, if you use the [managed API token validation](https://dev.office.com/docs/add-ins/outlook/use-the-token-validation-library) library, you will need to change the _hostUri_ parameter to match the modified [SourceLocation](http://dev.office.com/reference/add-ins/manifest/sourcelocation). Remember to change the Exchange identity token validation call back after you test the license check.
