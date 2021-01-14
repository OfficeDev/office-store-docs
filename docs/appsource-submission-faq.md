---
title: Microsoft AppSource submission FAQ
description: Answers the most common questions about submitting an add-in to Microsoft AppSource.
ms.date: 11/24/2020
localization_priority: Normal
---

# Microsoft AppSource submission FAQ

This topic answers common questions about submitting apps to Microsoft AppSource via Partner Center.

For the most current version of the certification policies, see [Certification policies](/legal/marketplace/certification-policies).

## How can I avoid errors when submitting my app to Microsoft AppSource?

To avoid common submission errors:

- Make sure that the version number on the submission form matches the version number in the app manifest.
    
    > [!NOTE]
    > Specify your app version using the following syntax: *a*  . *b*  . *c*  . *d*  Where *a*  is an integer between 1-9999, and each of *b*  , *c*  , *d*  are each integers between 0-9999. For example: 1.0.0.0 6.23.0.1.

- Make sure that all locations are SSL-secured (HTTPS).

- Make sure you that you specify an icon in your manifest, and that the icon is correctly sized and formatted. 

- Make sure your ID is unique. For example, do not create a manifest for a second add-in based on another add-in manifest you submitted without changing the ID in the new manifest.

- For Office Add-ins, make sure that you are using [manifest schema version 1.1](/office/dev/add-ins/overview/add-in-manifests). For information about updating your manifest to version 1.1, see [Update to the latest JavaScript API for Office library and version 1.1 add-in manifest schema](/office/dev/add-ins/develop/update-your-javascript-api-for-office-and-manifest-schema-version). 

- For Office Add-ins, make sure that you specify a support URL in the **SupportUrl** element of your Office Add-in manifest. Your support URL should be a publicly available webpage, and should not require authentication. You cannot use personal social media pages or GitHub repositories for the support URL. You also can't use links to files hosted online such as a Word document on OneDrive, DropBox or Google Docs.

-  For all apps, make sure that your manifest is valid against the schema. For schema validation information, see [Schema reference for Office Add-ins manifests (v1.1)](/office/dev/add-ins/overview/add-in-manifests) or [Schema reference for manifests of SharePoint Add-ins](/sharepoint/dev/schema/schema-reference-for-manifests-of-sharepoint-add-ins).

- Make sure your app is tested and is fully functional.

- Make sure your SharePoint Add-ins specify their supported locales. 
    
  If you don't specify supported locales, your app will not be accepted by Microsoft AppSource. For details, see [Locale support information is required for all add-ins](/archive/blogs/officeapps/locale-support-information-is-required-for-all-apps-in-the-sharepoint-store).

- Make sure your OAuth client IDs match
    
  If your SharePoint Add-in accesses services using OAuth, make sure the OAuth client ID that you created matches the client ID in your app manifest.

- Your SharePoint Add-in package must conform to the Open Packaging Convention.

- Make sure that you submit a privacy link. 

- Make sure that any video links you submit actually go to a video file or a page that includes a video.

- If your Office Add-in is available on iOS, do not include "app" in the **Add-in Title** or **Add-in Short Description**.

## If I make updates to my submission, when do I have to resubmit it to Microsoft AppSource?

If you make updates to the web service for your app, you do not have to resubmit it. However, if you make changes to any items or data you submitted via Partner Center, such as the manifest, screenshots, or icon, you need to resubmit it so that Microsoft AppSource can implement those changes. You must resubmit apps with an updated manifest that includes a new version number. You must also make sure to update the version number in the submission form to match the version number of the new manifest.

<a name="bk_q3"> </a>

## What happens when I update my app to a new version in Microsoft AppSource?

The following is the update process for Office Add-ins:

- You submit your revised add-in and add-in manifest to Microsoft AppSource via [Partner Center](https://partner.microsoft.com//dashboard/office/overview). The revised add-in goes through the validation process, and when approved, is made available in Microsoft AppSource.

> [!IMPORTANT]
> If your submission is an update and you schedule the availability of your app to a date in the future, your existing app will be unpublished from Microsoft AppSource until the release date you specified.

- You can choose to continue to offer the previous version of your app in Microsoft AppSource, or you can unpublish the previous version.

- When an existing customer launches the updated app for the first time, a notification appears either in the task pane or the body of the document that prompts the user to update their app. When the user chooses **Update**, the latest version of the app launches.

If the updated version includes new permissions, the user must consent to them.

> [!NOTE]
> You cannot have two or more versions of the same app in Microsoft AppSource at the same time, because each app has a unique asset ID. If you publish an updated version of your app without unpublishing a previous version, you will have two listings and will potentially split your customer base.

Updates to SharePoint Add-ins are handled by the license-management tools that are part of the SharePoint Add-in catalog. For more information, see [SharePoint Add-ins update process](/sharepoint/dev/sp-add-ins/sharepoint-add-ins-update-process).

## Can I submit a paid app to Microsoft AppSource?

All apps submitted to Microsoft AppSource via Partner Center are free. You can monetize your app through the Microsoft Commercial Marketplace; for details, see [Monetize your add-in](monetize-addins-through-microsoft-commercial-marketplace.md). 

## How do I reference the JavaScript APIs for Office in my apps?

If your app uses the JavaScript APIs for Office, you must [reference the Microsoft-hosted Office.js file from its CDN URL](/office/dev/add-ins/develop/referencing-the-javascript-api-for-office-library-from-its-cdn). Don't include a copy of the Office.js file in your app, or reference a copy of the file hosted elsewhere.

## Why do my apps have to be SSL-secured?

Apps that are not SSL-secured (HTTPS) generate insecure content warnings and errors during use. For this reason, all apps submitted to Microsoft AppSource are required to be SSL-secured.

## How do I declare language support?

Two aspects of your submission relate to supported languages:

1. The languages you declare in your app package or manifest. You declare which languages your app supports differently depending on type:

   - For SharePoint Add-ins, declare language support by using the [SupportedLocales element (PropertiesDefinition complexType) (SharePoint Add-in Manifest)](/sharepoint/dev/schema/supportedlocales-element-propertiesdefinition-complextypesharepoint-add-in-manif), in the add-in manifest within the add-in package. For more information, see [Explore the app manifest structure and the package of a SharePoint Add-in](/sharepoint/dev/sp-add-ins/explore-the-app-manifest-structure-and-the-package-of-a-sharepoint-add-in).

   - For Office Add-ins that aren't dictionaries, declare language support by using the **DefaultLocale** and **Override** elements in your manifest. For more information, see [Localization for Office Add-ins](/office/dev/add-ins/develop/localization).

   - For Office Add-ins that are dictionaries, you can also use the **TargetDialects** element within the add-in manifest.

2. In Partner Center, you can select languages on the **Store listings** page.

    > [!NOTE]
    > You can declare more languages in your app package than are available for submission in Partner Center.

## See also

- [Submit your solution to Microsoft AppSource](use-partner-center-to-submit-to-appsource.md)