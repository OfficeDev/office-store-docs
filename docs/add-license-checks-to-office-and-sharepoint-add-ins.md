
### To load a test license from the file system


1. Create a folder that is accessible via a UNC path (c:\ _folder_ or \\ _server_\ _share_).
    
 
2. Add the manifest file for your add-in to the folder (the file name must have an .xml extension). The following code shows an example manifest file for a content add-in.
    

3. Add the token file to the folder. The token file name must be the same as the manifest file name and must have a .tok file extension. The following code shows an example token file. Refer to the  [Office and SharePoint Add-in license XML schema structure](office-and-sharepoint-add-in-license-xml-schema-structure.md) for details about the attribute values you can set in the **t** element of the token file.
    
  ```XML
  <r>
  <t 
    aid="WA900006056"
    pid="{4FB601F2-5469-4542-B9FC-B96345DC8B39}"
    cid="32F3E7FC559F4F49"
    et="Trial"
    ad="2012-01-12T21:58:13Z"
    ed="2012-06-30T21:58:13Z"
    sd="2012-01-12T00:00:00Z" 
    te="2012-06-30T02:49:34Z"
    test="true"/>
  <d>VNNAnf36IrkyUVZlihQJNdUUZl/YFEfJOeldWBtd3IM=</d>
</r>
  ```

4. Create an entry in the registry that points to the manifest under one of the following paths:
    
      -  `HKEY_CURRENT_USER\Software\Microsoft\Office\15.0\Wef\Developer` (Office 2013)
    
 
  -  `HKEY_CURRENT_USER\Software\Microsoft\Office\16.0\Wef\Developer` (Office 2016)
    
 

    You can use a .reg file, as shown in the following example. (Note that the name field,  `"entry1"` and the .xml file name in this example are arbitrary.)
    


  ```
  Windows Registry Editor Version 5.00

[HKEY_CURRENT_USER\Software\Microsoft\Office\16.0\Wef\Developer]
"entry1"="C:\\folder\\AppFile.xml"
  ```


## Add license checks to your SharePoint Add-in
<a name="bk_add"> </a>

You can create test licenses and import them into your SharePoint deployment. To assist in testing the add-in license checking code, SharePoint enables you to upload up to ten test licenses per deployment. These test licenses are XML fragments that conform to the  [Office and SharePoint Add-in license XML schema structure](office-and-sharepoint-add-in-license-xml-schema-structure.md).
 

 
To import test licenses, use the  **ImportAppLicense** method. To call this method, the caller must be one of the following:
 

 

- An administrator of the site collection being called.
    
 
- An administrator of the tenancy into which the license is imported, if the SharePoint deployment into which the license is imported is a tenancy.
    
 
- A farm administrator.
    
 
After you import the test licenses, they appear in the SharePoint UI, and you can manage, assign, and delete them.
 

 
For test licenses, you don't have to specify the deployment ID in the add-in license XML. The  **ImportAppLicense** method supplies the correct deployment ID to the license token XML.
 

 

### Code example: Import a test license token into SharePoint
<a name="bk_example_import"> </a>

The following example takes a test add-in license token and imports it into the specified SharePoint installation.
 

 

```C#
// For this example to work, you must add a reference in your project to Microsoft.SharePoint.Client.dll and Microsoft.SharePoint.Client.Runtime.dll.

string rawXMLEntitlementToken = <token that you want to import>;
string webUrl = "http://localhost" // This localhost URL should be replaced with the URL of any site within the tenancy into which 
// You want to import the license.

using (ClientContext ctx = new ClientContext(webUrl))
{
    Microsoft.SharePoint.Client.Utilities.Utility.ImportAppLicense(
        context: ctx,
        licenseTokenToImport: rawXMLEntitlementToken,
        contentMarket: "en-US", // Replace this with whatever content market you want
        billingMarket8; "US", // Replace this with whatever billing market you want
        appName: "add-in Name", // Replace this with the name of the app
        iconUrl: "http://www.office.com", // Replace this with the URL of the icon of the add-in (as it appears on Office Store),
// Or you can simply leave the URL blank.
        providerName: "Provider Name"); // Replace this with the name of the provider of the app

    ctx.ExecuteQuery();
}

```


### Implement add-in license checks in your SharePoint Add-in code
<a name="SP15Implementlicense_bk_implement"> </a>

Identify where in your add-in you want to check for a valid license or other license information. For example, when the add-in launches, or when the user goes to a specific page or accesses certain features. Add code at these points that queries your SharePoint deployment for the license token, and then passes that token to the Office Store verification web service for validation.
 

 
To retrieve the license token from SharePoint, use the  **GetAppLicenseInformation** method. This method returns all licenses for the specified add-in that apply to the user, based on the add-in product ID in the manifest file.
 

 
If multiple licenses are purchased for the same add-in by using different Microsoft accounts, the licenses are returned in the following order of priority:
 

 

- Paid
    
 
- Free
    
 
- Unexpired Trial
    
 
- Expired Trial
    
 
The  **GetAppLicenseInformation** method does not return licenses with expired or preserved tokens. Preserved tokens are the license tokens that cannot be renewed automatically by SharePoint. To remain valid, preserved tokens must be renewed manually by having the purchaser sign in to the Office Store.
 

 

#### Code example: retrieve add-in license tokens

The following example retrieves all the add-in licenses for the current user as a collection that can be iterated through.
 

 

```C#
// For this example to work, you must add a reference in your project to Microsoft.SharePoint.Client.dll and Microsoft.SharePoint.Client.Runtime.dll.
// For this API to work, the SharePoint deployment you are calling must be able to communicate with ACS to validate OAuth tokens.

string webUrl = "http://localhost" // This localhost URL should be replaced with the URL of the add-in web or host web of the app.
    // If you are redirected from the add-in web to the third-party side executing this code
    // in the code-behind, you can get the add-in web URL with 
    // HttpContext.Current.Request.QueryString["AppWebUrl"].

productId = new Guid(<product ID of the app>);
using(ClientContext ctx = new ClientContext(webUrl))
{
    ClientResult<AppLicenseCollection> licensecollection = Microsoft.SharePoint.Client.Utilities.Utility.GetAppLicenseInformation(ctx, productId);
    ctx.ExecuteQuery();
}

```

By the end of this example,  `licensecollection` includes all the add-in licenses for the current user as a collection of **AppLicense** objects. You can use the **RawXMLLicenseToken** property to access the license token XML. So, for example, to access the license token for the first add-in license token in the collection, you use `licensecollection.Value[0].RawXMLLicenseToken`.
 

 

 **Important**  Do not to parse or otherwise manipulate the add-in license token string before passing it to the Office Store license verification web service for verification. Although the add-in license token is structured as an XML fragment, for purposes of validation the Office Store verification web service treats the token as a literal string. The Office Store verification web service compares the contents of the <t> element to the value of the <d> element, which is an encrypted signature derived from the literal string contained in the <t> element. Any reformatting of the license token, such as adding white space, tabs, or line breaks, change the literal value of the <t> element and cause the license verification check to fail. 
 


#### Validating the add-in license token

After you retrieve the appropriate add-in license token, pass that token to the Office Store verification web service for validation. The verification service is located at the following URL:
 

 
 `https://verificationservice.officeapps.live.com/ova/verificationagent.svc`
 

 
The verification service has a single method,  **VerifyEntitlementToken**, which takes the add-in license token as a parameter and returns a  **VerifyEntitlementTokenResponse** object that contains the properties of the license. The **IsValid** property specifies whether the license token is valid, and other properties, such as **ProductId** and **EntitlementType**, contain information about the various license attributes.
 

 
The Office Store license verification web service also supports verifying add-in license tokens by using REST calls. To verify an add-in license by using REST, use the following syntax:
 

 
 `https://verificationservice.officeapps.live.com/ova/verificationagent.svc/rest/verify?token={token}`
 

 
Where  `{token}` is the add-in license token, encoded by a method that complies with RFC 2396. For example, the **encodeURIComponent()** function in JavaScript, or the **Uri.EscapeDataString** method in the .NET Framework. The Office Store verification service does not support being called from client-side code.
 

 

 **Note**  If you're hosting your add-in pages on SharePoint, you can use the SharePoint web proxy to make JavaScript calls to the Office Store verification service. However, for security reasons we strongly recommend that you use only server-side code to query the Office Store verification web service.
 


 **Caution**  Do not store the license token uby sing a service or application that adds a byte order mark (BOM) to the license token string. Including this character in the license token passed to the verification service will cause the license check to fail. If you do use an application that adds a BOM to the token, you must remove this character before passing the license token to the verification service.
 


### Take action based on the SharePoint Add-in license
<a name="SP15implementlicense_bk_take"> </a>

Add code to your add-in that takes the appropriate actions, based on whether the license is valid and, if it is valid, any other license information that is important to you. For example, add code that enables the user to access certain features if their license is for the paid version, but not if their license is for the trial version of the add-in.
 

 

### Add code to block test licenses
<a name="SP15implementlicense_bk_add"> </a>

Finally, after you finish testing your add-in and are ready to move it to production, you need to add code to the license checks so that the add-in no longer accepts test licenses. This prevents users from using test licenses to access your add-in on their SharePoint deployment.
 

 
After you pass the license token to the verification service's  **VerifyEntitlementToken** method, you can use the **VerifyEntitlementTokenResponse** object returned by that method to access the license properties. For test licenses, the **IsTest** property returns **true** and the **IsValid** property returns **false**.
 

 

### Code example: SharePoint Add-ins licensing checking
<a name="bk_example"> </a>

The following example retrieves an add-in's license token from the SharePoint deployment and passes the token to the Office Store verification service for validation. The example catches a variety of possible errors if the verification fails. If the verification succeeds, it builds a string from the various license properties. Finally, the code provides logic for specifying the level of functionality based on the license type: Free, Paid, or Trial. 
 

 
This example requires a reference to  **Microsoft.SharePoint.Client.Utilities**, and a web service reference to the Office Store verification service.
 

 



```C#
//Get the license token XML from SharePoint.
this.rawToken = GetLicenseTokenFromSP(this.productId, this.clientcontext);

//Call the Office Store verification service.
VerifyLicenseToken(this.rawToken);

private string GetLicenseTokenFromSP(Guid productId, ClientContext clientContext)
{
    //Get the license from SharePoint.
    ClientResult<AppLicenseCollection> licenseCollection = Utility.GetAppLicenseInformation(clientContext, productId);
    clientContext.Load(clientContext.Web);
    clientContext.ExecuteQuery();

    foreach (AppLicense license in licenseCollection.Value)
    {
        //Just get the first license token for now.
        rawLicenseToken = license.RawXMLLicenseToken;
        break;
    }
    return (rawLicenseToken);
}

private void VerifyLicenseToken(string rawLicenseToken)
{    
    if (string.IsNullOrEmpty(rawLicenseToken))
    {
        licVerifyEndPoint.Text = "There is no valid license for this user in SharePoint (OR) license cannot be obtained due to some error - check ULS.";
        return;
    }

    VerificationServiceClient service = null;
    VerifyEntitlementTokenResponse result = null;
    VerifyEntitlementTokenRequest request = new VerifyEntitlementTokenRequest();
    request.RawToken = rawLicenseToken;
    lblSPLicenseText.Text = System.Web.HttpUtility.HtmlEncode(request.RawToken);   

    try
    {
        service = new VerificationServiceClient();
        result = service.VerifyEntitlementToken(request);
    }
    catch (EndpointNotFoundException)
    {
        licVerifyEndPoint.Text = "Cannot access verification service endpoint";
    }
    catch (FaultException<ServiceUnavailableFault>)
    {
        licVerifyEndPoint.Text = "Error: entitlement verification service is unavailable.";
    }
    catch (FaultException<ServiceInternalErrorFault> internalFault)
    {
        licVerifyEndPoint.Text = "Error: entitlement verification service failed. Details: " + internalFault.Detail.Message;
    }
    catch (Exception exception)
    {
        licVerifyEndPoint.Text = "Error: entitlement verification service failed. Details: " + exception;
    }

    if (result != null &amp;&amp; result.AssetId !=null)
    {
        string licenseDetails = string.Format("Asset Id: {0}; Product Id: {1}; License Type: {2}; Is Valid: {3}; License Acquisition Date: {4}; License Expiry Date: {5}; IsExpired: {6}; IsTest: {7}; IsSiteLicense: {8}; Seats: {9}; TokenExpiryDate: {10}",
                result.AssetId, result.ProductId, result.EntitlementType, result.IsValid, result.EntitlementAcquisitionDate, result.EntitlementExpiryDate, result.IsExpired, result.IsTest, result.IsSiteLicense, result.Seats, result.TokenExpiryDate);

        if (result.EntitlementType.ToUpper() == "FREE")
        {
          //Allow basic functionality
        }
        else if (result.EntitlementType.ToUpper() == "PAID")
        {
          //Allow all functionality
        }
        else //trial
        {
          //Allow limited functionality
        }
    }
            else
    {
        licVerifyEndPoint.Text = "Verification service didn't return any results";
    }
}

```


## Additional resources
<a name="bk_addresources"> </a>


-  [License your Office and SharePoint Add-ins](license-your-office-and-sharepoint-add-ins.md)
    
 
-  [How licenses work for Office and SharePoint Add-ins](how-licenses-work-for-office-and-sharepoint-add-ins.md)
    
 
-  [Office and SharePoint Add-in license XML schema structure](office-and-sharepoint-add-in-license-xml-schema-structure.md)
    
 
-  **VerificationSvc**
    
 
-  [SharePoint 2013 code sample: Import, validate, and manage add-in licenses](http://code.msdn.microsoft.com/SharePoint-2013-Import-f5f680a6)
    
