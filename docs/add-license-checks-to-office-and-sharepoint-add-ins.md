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

## Implement license checks in the Office Add-in code
<a name="bk_implement"> </a>

Think about where in your add-in you want to check for a valid license or other license information. For example, when the add-in launches, or when the user goes to a specific page or accesses certain features.
 

 
Before you can check the license, you'll have to acquire and cache the add-in license token. When a user opens an Office Add-in, the Office application requests the add-in home page. The Office application makes the HTTP request for the home page, including the license token as a query string parameter on the page's URL.
 

 
For example, suppose your add-in home page has the following URL:
 

 
 `http://myApp/index.html`
 

 
The Office application calling that URL would add the following query string to it and then pass the URL:
 

 
 `http://myApp/index.htm?et= PAByAD4APAB0ACAAYQBpAGQAPQAiAFcAQQAxADAAMgA4ADkAOQA1ADYANgAiACAAcABpAGQAPQAiADMAZAAyADgANwAwADcAYQAtAGYAYwBjAGUALQA0ADUAMQA3AC0AYQBjADYAZQAtAGMAYQAwAGEAZABkADYAMwA3ADMAYQBhACIAIABjAGkAZAA9ACIAMgAzAEEANwBFAEIAOABBADQAQwA0ADcARgA1AEEAMgAiACAAdABzAD0AIgAwACIAIABzAGwAPQAiAHQAcgB1AGUAIgAgAGUAdAA9ACIARgByAGUAZQAiACAAYQBkAD0AIgAyADAAMQAyAC0AMAA1AC0AMgAyAFQAMQA4ADoAMQAyADoAMgAzAFoAIgAgAHMAZAA9ACIAMgAwADEAMgAtADAANQAtADIAMgAiACAAdABlAD0AIgAyADAANgA3AC0AMAAyAC0AMgAzAFQAMQA4ADoAMQA0ADoAMAAwAFoAIgAgAC8APgA8AGQAPgAyADIAWABLAEEAdgA0ADMAQgBtAHMAcwByADAAcgBxADUANQBGAHUAdgBpAFUAVgBSAGkAVgBLAFMASQBEAGcAeAAyAHAAMgA0AFoAZwBzAGwANgBNAD0APAAvAGQAPgA8AC8AcgA%2bAA%3d%3d`
 

 
The query string parameter— _et_—specifies a base-64 and URL-encoded version of the add-in license token.
 

 
For Outlook add-ins, the  *et*  query parameter string is only URL-encoded, and **not** base-64 encoded.
 

 
For example, the source location modified to include a test token for an Outlook add-in would look like this: 
 

 
 `https://myApp/index.htm?et=%3Cr%20v%3D%221%22%3E%3Ct%20aid%3D%22WA104108294%22%20pid%3D%22463eafac-c123-45fe-bd21-b1b120b4c12b%22%20cid%3D%223BEC2F1C0124D801%22%20did%3D%22CONTOSO.COM%22%20ts%3D%221%22%20et%3D%22Paid%22%20ad%3D%222013-08-29T21%3A38%3A14Z%22%20sd%3D%222013-09-17%22%20te%3D%222013-12-23T09%3A10%3A42Z%22%20test%3D%221%22%20ss%3D%220%22%20%2F%3E%3Cd%3E7uM9j2%2FYZJeZrrm2TLjXufQlwkAXkq2RqjowBP9fAjo%3D%3C%2Fd%3E%3C%2Fr%3E`
 

 

 **Important**  For security reasons, if you are licensing your Office Add-in, we strongly recommended you specify an HTTP Secure ( `https://`) URL for your add-in home page.
 

To perform add-in license checks, include code that extracts the license token from the URL and caches it, so that the add-in can pass the token to the verification service later when you want to actually validate the license.
 

 
For example, the following code extracts the token from the URL, decodes the token, and formats it as a string:
 

 



```C#
// Obtains token URL
string token = Request.Params["et"].ToString(); 

// Applies base64 decoding of the token to get a decoded token.
byte[] decodedBytes = Convert.FromBase64String(token); 
string decodedToken = Encoding.Unicode.GetString(decodedBytes);
```


 **Note**  The decoding will throw an error if the token contains white space. Make sure that you handle white space between characters within the token.
 

To help maximize the reach and adoption, task pane and content add-ins allow anonymous access. Microsoft does not require that a user be signed into Office with their Microsoft account in order to activate task pane and content add-ins. The license token will be passed as part of the initial HTTP request only if the user is signed in with their Microsoft account.
 

 
For task pane and content add-ins, your code should first test for the presence of the  *et*  parameter in the HTTP request. If it is not present, you should treat the user as anonymous, and present the appropriate user experience.
 

 
For more information, see  [Add-in license tokens and anonymous access for Office Add-ins](license-your-office-and-sharepoint-add-ins.md#bk_anonymous) in [License your Office and SharePoint Add-ins](license-your-office-and-sharepoint-add-ins.md).
 

 

 **Important**  Do not to parse or otherwise manipulate the add-in license token string before passing it to the Office Store verification web service for verification. While the add-in license token is structured as an XML fragment, for purposes of validation the Office Store verification web service treats the token as a literal string. The Office Store verification web service compares the contents of the <t> element to the value of the <d> element, which is an encrypted signature derived from the literal string contained in the <t> element. Any reformatting of the license token, such as adding white space, tabs, or line breaks, will change the literal value of the <t> element and therefore cause the license verification check to fail. Also, do not store the license token using a service or application that adds a byte order mark (BOM) to the license token string. Including this character in the license token passed to the verification service will cause the license check to fail. If you do use an application that adds a BOM to the token, you must remove this character before passing the license token to the verification service.
 

When the add-in needs to perform a license check, pass the license token to the Office Store license verification web service for validation. The verification service is located at the following URL:
 

 
 `https://verificationservice.officeapps.live.com/ova/verificationagent.svc`
 

 
The verification service has a single method,  **VerifyEntitlementToken**, that takes the license token as a parameter and returns a  **VerifyEntitlementTokenResponse** object that contains the properties of the license. The **IsValid** property specifies whether the license token is valid. Other properties, such as **ProductId** and **EntitlementType**, contain information about the various license attributes.
 

 
The Office Store license verification web service also supports verifying add-in license tokens by using REST calls. To validate an add-in license by using REST, use the following syntax, where  `{token}` is the add-in license token, encoded by a method that complies with RFC 2396. For example, the **encodeURIComponent()** function in JavaScript, or the **Uri.EscapeDataString** method in the .NET Framework:
 

 
 `https://verificationservice.officeapps.live.com/ova/verificationagent.svc/rest/verify?token={token}`
 

 
Calling the Office Store verification service from client-side code is not supported. You must use server-side code to query the Office Store verification web service.
 

 

## Add code for the action the Office Add-in takes, based on its license
<a name="bk_take"> </a>

Add code to your add-in that takes the appropriate action, based on whether the license is valid and, if it is valid, based on any other license information that is important to you. For example, code that enables the user to access certain features if the user's license is for the paid version, but not the trial version.
 

 

## Add code to block the Office Add-in from accepting test licenses
<a name="bk_add"> </a>

After you finish testing your add-in and you're ready to move it to production, add code to the license checks in your add-in so that it no longer accepts test licenses. This prevents users from using test licenses to access your add-in.
 

 
After you pass the add-in license token to the verification service's  **VerifyEntitlementToken** method, use the **VerifyEntitlementTokenResponse** object returned by that method to access the license properties. For test licenses, the **IsTest** property returns **true** and the **IsValid** property returns **false**.
 

 

 **Note**  For Outlook add-ins, make sure that you remove the  *et*  parameter, which represents the test license token, from all **SourceLocation** elements in your add-in manifest file.
 


## Code example: Check the Office Add-in license by retrieving and validating its add-in license token
<a name="bk_add"> </a>

The following example shows the basic logic flow of retrieving and validating the license token for a content or task pane add-in: 
 

 

1. The code retrieves the URL query string parameter,  `et`, which contains the encoded license token. 
    
 
2. The code uses a custom function to decode the license token and convert it from base-64 to a string format that the Office Store verification service accepts. 
    
     **Note**  For Outlook add-ins, the  *et*  query parameter string is only URL-encoded, and **not** base-64 encoded. To use this example with an Outlook add-in, remove the code that converts the token from base-64 encoding.
3. The code passes the token in string format to the verification service for validation. After the verification service returns a  **VerifyEntitlementTokenResponse** object that represents the validation results, the code can access the object's properties that contain attributes of the license token.
    
 
In this example, the code prints out the user ID of the add-in user and whether the license token is a test token.
 
