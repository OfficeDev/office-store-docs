---
title: License your Office and SharePoint Add-ins
description: Design your add-in user experience with licensing in mind, add license checks to your code, and use license query and validation flows.
ms.date: 1/11/2018
---

# License your Office and SharePoint Add-ins

The licensing framework for Office and SharePoint Add-ins gives you a way to include code in your add-ins to verify and enforce their legal use. You can restrict access to your add-ins to only those users who have a valid license, or specify which features are available, how the add-in behaves, or other logic, based on the properties of that license. If you plan to sell your add-in, you should build in logic that uses the licensing framework to determine whether a user has a valid license for the add-in, and give access to its features based on the license's properties.

The add-in license framework itself does not enforce add-in licenses. It's a structure in which you can add code to your add-ins to retrieve and then act on license information.

The add-in license framework applies only to add-ins acquired directly from the AppSource site or in-product experience, or add-ins from AppSource that are made available in an add-in catalog hosted on SharePoint. Add-ins made available in other ways—such as from a file system location, or uploaded directly to an add-in catalog hosted on SharePoint—cannot use the add-in license framework.

The Office and SharePoint add-in license framework includes:

- AppSource—a site where users can acquire licensed Office and SharePoint Add-ins. AppSource handles payment and issues licenses.
- Storage of licenses, and renewal of add-in license tokens.
- APIs you can use to get license information.
- A web service you can use to verify whether a license is valid.
- Specifically for SharePoint Add-ins, SharePoint provides an administration user interface for add-in license management, where an add-in purchaser can assign the license to a user, and also delegate to other users how the license is managed.
- Specifically for Outlook add-ins, Exchange provides the Exchange Administration Console, where administrators can purchase and manage Outlook add-ins for their organization. 

The add-in license framework provides a way for you to customize add-in access and behavior based on license information. The following sections outline the general pattern for performing add-in license checks.

## Design your add-in user experience with licensing in mind

If you plan on using the licensing framework, you should design your add-in with this in mind. Decide what user experience you want to control or customize based on license information. For example, you might want to:

- Check that the user has a valid license each time the user launches the add-in.
- Make only certain functionality in your add-in available if the user has a trial license. 
- For task pane or content add-ins, offer the user a different experience based on whether they access your add-in anonymously, or are signed in with a valid license.

## Add license checks to your add-in code

For each experience that you want to customize based on license information, add code for that event that performs a license check. The license check consists of determining if the license token is present, and if it is, validating that license token.

### Retrieve the license token, if present

Your license checking code should determine if the add-in license token is present, and retrieve it if it is.

- For content and task pane Office Add-ins, when the add-in is launched and the Office application requests the add-in home page, the Office application passes the license token as a query parameter in the HTTP request. The add-in code must extract and cache this information, so that the license checking code in the add-in can later access it.

  If the user is not signed in to their Microsoft account, the Office application requesting the add-in home page does not append the license token parameter. For more information, see the next section [Add-in license tokens and anonymous access for Office Add-ins](#bk_anonymous).
    
- For paid Outlook add-ins, Exchange appends the app's source location URL with a license token as a query parameter of the URL, and then transmits the add-in manifest to Outlook. When the add-in is launched, Outlook passes the license token as a query parameter in the HTTP request. The add-in code must extract and cache this information so that the license checking code in the add-in can later access it.

- For SharePoint Add-ins, the app's license checking code queries the SharePoint deployment for the license token.

<a name="bk_anonymous"> </a>
### Add-in license tokens and anonymous access for Office Add-ins

To help maximize the reach and adoption of your add-ins, as of Office 2013, Service Pack 1, Microsoft no longer requires that a user be signed in to Office with their Microsoft account to activate Office Add-ins. As of Office 2013, Service Pack 1, the add-in license token is passed as part of the initial HTTP request only if the user is signed in with their Microsoft account.

If the user is not signed in to their Microsoft account, the Office application requesting the add-in home page does not append the license token parameter. Therefore, you must include code in your add-in that determines whether the license token is present on each HTTP request for the app's home page. If it is not, your add-in can treat the request as coming from an anonymous user, and present the UI and functionality that you decide is appropriate. Use the add-in licensing framework to customize what your add-in presents to users who are not signed into their Microsoft accounts. For example, your add-in could present UI that provides more information about your add-in, a link to your add-in's AppSource listing, a reduced set of functionality, or other relevant material. 

|**Add-in license type**|**Recommended UX when the user is anonymous (license token is not present)**|
|:-----|:-----|
|Free|No change in behavior; add-in can function the same. However, if you rely on the license token to determine user identity<br/>of your free add-in, you might want to provide a notice to the user asking them to sign in to Office with a Microsoft account<br/>to get the full benefits of your add-in.|
|Trial|Provide the same trial add-in experience when the user is anonymous. If you rely on the license token to determine user identity<br/>of your trial add-in, you might want to provide a notice to the user asking them to sign in to Office with a Microsoft account<br/>to get the full benefits of your add-in.|
|Paid|If your add-in supports only paid licenses (that is, it doesn't provide a trial experience), you should present the user with<br/>information about your add-in, rather than a functional add-in, along with a hyperlink to your add-in's AppSource listing page.<br/>This way users are aware of your add-in and encouraged to purchase it.|

By default, if your add-in task pane or content add-in does not perform this licensing check, your add-in presents the same UI and functionality to anonymous users as it does to licensed users.

### Validate the license

After the add-in receives the add-in license token, the add-in must pass it to the AppSource verification web service to determine that the license is valid and the information it contains is accurate. The verification service returns whether the license is valid and the license attribute values. The add-in code can then take appropriate action, based on whether the license is valid and on the license information.

The AppSource verification service does not support being called from client-side code.

- For Office Add-ins, you are required to use server-side code to query the AppSource verification web service.
- For SharePoint Add-ins, if you are hosting your add-in pages on SharePoint, you can use the SharePoint web proxy to make JavaScript calls to the AppSource verification service. However, for security reasons, we strongly recommend that you only use server-side code to query the AppSource verification web service.

### Take action based on license properties

Finally, add code to your add-in that takes the desired action based on the properties of the user's add-in license. This could include displaying different UI based on subscription status, disabling certain functionality for trial licenses, or any other customization you want to make based on the license properties. 

## Understand add-in license query and validation flows

### Office add-in license verification process

Figure 1 shows the add-in license query and validation process for Office Add-ins.

> [!NOTE]
> Task pane and content add-ins allow anonymous access. If the user is not signed in to their Microsoft account, the Office application requesting the add-in home page does not append the license token parameter. For details, see the section [Add-in license tokens and anonymous access for Office Add-ins](#bk_anonymous).

*Figure 1. Office add-in license verification process*

![Office add-in license verification process](images/office15-add-in-license-verification.png)

1. The user launches the add-in.
2. The Office application that hosts the add-in requests the home page.
3. The Office application appends the add-in license token to the HTTP request as a query string parameter.
4. The add-in code extracts and caches the license token.
5. When the add-in needs to verify the license token, it uses server-side code to pass the token to the AppSource verification service.
6. The verification service returns whether the license token is valid, and if it is, also returns the license properties.
7. The add-in can then take action, based on the validity of the license and its properties.

### SharePoint add-in license verification process

Figure 2 shows the add-in license query and validation process for SharePoint Add-ins. 

*Figure 2. SharePoint add-in license verification process*

![SharePoint add-in license verification process](images/sp15-add-in-license-verification.png)

1. The user launches the add-in from within SharePoint.
2. This launches the add-in code in the cloud.
3. When the add-in needs to verify a user's add-in license, it uses server-side code to query SharePoint, via the client object model, for the add-in license token.
4. It then passes that token to the AppSource verification service.
5. The verification service returns whether the license token is valid, and if it is, also returns the license properties.
6. The add-in can then take action, based on the validity of the license and its properties.

## See also

- [How licenses work for Office and SharePoint Add-ins](how-licenses-work.md)
- [Add license checks to Office and SharePoint Add-ins](add-license-checks-to-office-and-sharepoint-add-ins.md)
- [Implement licensing to upsell your Office Add-in services](implement-licensing-for-add-in-services.md)
- [Office and SharePoint Add-in license XML schema structure](add-in-license-schema.md)
- [Decide on a pricing model for your AppSource submission](decide-on-a-pricing-model.md)
- [VerificationSvc namespace](https://msdn.microsoft.com/en-us/library/verificationsvc.aspx)
- [SharePoint 2013 code sample: Import, validate, and manage app licenses](https://code.msdn.microsoft.com/SharePoint-2013-Import-f5f680a6)
- [Make your solutions available in AppSource and within Office](submit-to-the-office-store.md)
    
