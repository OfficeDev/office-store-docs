---
title: Microsoft 365 Store step-by-step submission guide
description:  Use this step-by-step submission guide to submit your app to the Microsoft stores. 
localization_priority: Normal
ms.author: mingshen
ms.date: 9/13/2021
---

# Store step-by-step submission guide

This article is a step-by-step guide that will detail how to submit your app to Microsoft 365 Stores.

>[!TIP]
>We recommend reading our [pre-submission checklist](./checklist.md) before reading this store submission guide so you have all information ready to go for submission.

## Step 1: Select the type of app you are submitting

#### [Workspaces view](#tab/Workspaces-view)

1. Sign in to [Partner Center](https://partner.microsoft.com/dashboard/home).
1. Select the **Marketplace offers** tile.

    :::image type="content" source="./images/new/marketplace-offers-tile.png" alt-text="Illustrates Marketplace offers tile on the Partner Center Home page.":::

1. In the upper-left of the page, select **+ New offer** and then select the type of app you're submitting.

    :::image type="content" source="./images/new/new-offer-workspaces.png" alt-text="Illustrates the New offer list.":::

#### [Classic view](#tab/classic-view)

1. Sign in to [Partner Center](https://partner.microsoft.com/dashboard/home).
1. In the left-navigation menu, select **Office Store** > **Overview**.
1. Select **+ Create a new** and then select the type of app you're submitting.

![Overview tab selected in the Microsoft Partner Center menu](./images/new/step-select-type-of-add-in.png)

---

> [!NOTE]
> The example screenshots in this article show an Office Add-in, but the steps apply to Teams apps, SharePoint solutions, and so on.

## Step 2: Name your app

You’ll be prompted to enter a name for your app.

![Create a new Office Add-in menu with name submission box.](./images/new/step-2-name-addin.png)

## Step 3: Tell us about your Product Setup

- **Will your app be listed in the Apple Store?**
    If so, include your Apple ID. You'll see a warning that reminds you to enter this information on screen.
- **Does your app use Azure Active Directory or SSO (Azure AD/SSO)?**
    If so, select the box that asks about this.
- **Does your app require additional purchases?**
    If so, select the box that asks about this. You will see a warning that reminds you to fill in the notes box on the review and publish page in a later step with your test credentials so a tester can verify this.
- **Do you want to connect with your lead management CRM system?**
    If so, connect this system using the Connect link.

The following screenshot shows two yellow warning boxes that remind you to fill in your Apple ID and provide test credentials.

#### [Workspaces view](#tab/Workspaces-view)

:::image type="content" source="./images/new/step-3-yellow-warnings-workspaces.png" alt-text="Illustrates the Product setup page.":::

#### [Classic view](#tab/classic-view)

![Yellow warning boxes remind you to fill in your Apple ID and to provide your test credentials.](./images/new/step-3-yellow-warnings.png)

---

## Step 4: Upload your manifest for package testing

#### [Workspaces view](#tab/Workspaces-view)

You will need to upload your manifest file to the grey box on this page.

![Your manifest needs to be uploaded to the grey box shown in this screenshot.](./images/new/step-4-packages-incomplete-upload-workspaces.png)

Remember to pre-test your package to prevent any unexpected failures in this step. Get information on all [the pre-testing manifest tools](/office/dev/add-ins/testing/troubleshoot-manifest#:~:text=%20To%20use%20a%20command-line%20XML%20schema%20validation,and%20replace%20XML_FILE%20with%20the%20path...%20More%20).

When your manifest is uploaded and is correct, you will receive a confirmation and see that manifest checks have passed.

![A completed manifest upload](./images/new/step-4-packages-complete-upload-workspaces.png)

#### [Classic view](#tab/classic-view)

You will need to upload your manifest file to the grey box on this page.

![Your manifest needs to be uploaded to the grey box shown in this screenshot.](./images/new/step-4-packages-incomplete-upload.png)

Remember to pre-test your package to prevent any unexpected failures in this step. Get information on all [the pre-testing manifest tools](/office/dev/add-ins/testing/troubleshoot-manifest#:~:text=%20To%20use%20a%20command-line%20XML%20schema%20validation,and%20replace%20XML_FILE%20with%20the%20path...%20More%20).

When your manifest is uploaded and is correct, you will receive a confirmation and see that manifest checks have passed.

![A completed manifest upload](./images/new/step-4-packages-complete-upload.png)

---

## Step 5: Define the metadata that will categorize your app in the store

In this step, you will add secure URL links to your support policies, privacy policies, and End User License Agreements (EULA). For more information on what your policies should look like, see [our submission checklist](./checklist.md).

The yellow warning seen in the following screenshots remind you that notes for certification are required.

#### [Workspaces view](#tab/Workspaces-view)

![Properties step with support information highlighted](./images/new/step-5-a-validation-workspaces.png)

The following image shows that to pass certification, your privacy policy must include:

- Information on your policies regarding user's personal information.
- Refer to the app or your service overall and NOT your website.
- Your service description must include the name of app submitted.
- A valid URL link that does not generate a 404 error.

A Terms of Use Policy is not considered a privacy policy. You must include a privacy policy that is separate from your Terms of Use policy.

![Privacy policy specifications](./images/new/step-5-b-validation-workspaces.jpg)

You will also need to include a EULA. If you provide your own EULA, the link you provide must be an https:// address. If you choose to use Microsoft's standard end user license agreement (EULA), you will need to confirm that you do not want to use your own agreement, as this cannot be reversed once your app is published.

![Warning stating if you use your own EULA you will need to confirm that you don't want to use your own agreement](./images/new/step-5-c-validation.jpg)

If you don't specify a support document link, you will be prompted to enter one.
<!--- [This doesn't appear to be true in the Workspaces UI]() --->
![Prompt asking you to insert a support document link](./images/new/step-5-d-support-warning.png)

#### [Classic view](#tab/classic-view)

![Properties step with support information highlighted](./images/new/step-5-a-validation.png)

The following image shows that to pass certification, your privacy policy must include:

- Information on your policies regarding user's personal information.
- Refer to the app or your service overall and NOT your website.
- Your service description must include the name of app submitted.
- A valid URL link that does not generate a 404 error.

A Terms of Use Policy is not considered a privacy policy. You must include a privacy policy that is separate from your Terms of Use policy.

![Privacy policy specifications](./images/new/step-5-b-validation.jpg)

You will also need to include a EULA. If you provide your own EULA, the link you provide must be an https:// address. If you choose to use Microsoft's standard end user license agreement (EULA), you will need to confirm that you do not want to use your own agreement, as this cannot be reversed once your app is published.

![Warning stating if you use your own EULA you will need to confirm that you don't want to use your own agreement](./images/new/step-5-c-validation.jpg)

If you don't specify a support document link, you will be prompted to enter one.

![Prompt asking you to insert a support document link](./images/new/step-5-d-support-warning.png)

---

## Step 6: Define your languages in Marketplace Listings

To create a store entry, on the **Marketplace listings page**, select **Manage additional languages**, select the language your app is available in, and then select **Update**.

![Marketplace listings step with arrow pointing toward Manage Additional Languages and the Add languages step with an arrow toward English](./images/new/step-6-define-your-languages.png)

## Step 7: For each language your app is available in, create your detailed store listing

Your store listing should include copy, icons, videos, and screenshots. For details, see [our submission checklist](./checklist.md).

Within the language option, you'll find where you can fill in your app's store listing information, such as your app description and images. In the **Language** column, select the language and then fill out the listing information.

![English has been added to the listing](./images/new/step-7-define-your-store-page-workspaces.png)

## Step 8: Decide on your availability date

#### [Workspaces view](#tab/Workspaces-view)

On the **Availability** page, schedule when your app will be available. Note it typically takes 4 to 6 weeks to complete an app submission and get it approved. On average, most apps require multiple submissions to pass our validation process, so follow our checklist carefully to reduce this time.

![Make my product available in a future market is selected in the Availability step](./images/new/step-8-set-availability-time-date-workspaces.png)

#### [Classic view](#tab/classic-view)

On the **Availability** page, schedule when your app will be available. Note it typically takes 4 to 6 weeks to complete an app submission and get it approved. On average, most apps require multiple submissions to pass our validation process, so follow our checklist carefully to reduce this time.  

![Make my product available in a future market is selected in the Availability step](./images/new/step-8-set-availability-time-date.png)

---

## Step 9:  Make sure you add your critical testing instructions

This final critical step requires you to include notes for certification. Provide any instructions for the reviewer who will be testing your app, including test accounts, license keys, and testing credentials.

If you indicated in a previous step that your app requires additional purchases, make sure you provide any information such as license keys that a review might need to evaluate your app.

The following image shows the **Notes for certification** box where you must provide information.

#### [Workspaces view](#tab/Workspaces-view)

![Provide your testing credentials in the red box](./images/new/step-9-certification-notes-workspaces.png)

In addition to _Notes for certification_, you can optionally provide additional detailed instructions for the reviewers by uploading a PDF file to the **Additional certification info** page. This option provides the following benefits:

- Enables you to include images to improve the clarity of the instructions
- The PDF file is uploaded, saved, and persisted for subsequent submissions

The following image shows where you can optionally upload a PDF file of instructions for reviewers.

:::image type="content" source="./images/new/additional-cert-info-workspaces.png" alt-text="Illustrates the Additional Certification Info page in Parter Center.":::

#### [Classic view](#tab/classic-view)

![Provide your testing credentials in the red box](./images/new/step-9-certification-notes.png)

In addition to _Notes for certification_, you can optionally provide additional detailed instructions for the reviewers by uploading a PDF file to the **Additional certification info** page. This option provides the following benefits:

- Enables you to include images to improve the clarity of the instructions
- The PDF file is uploaded, saved, and persisted for subsequent submissions

The following image shows where you can optionally upload a PDF file of instructions for reviewers.

:::image type="content" source="./images/new/additional-cert-info.png" alt-text="Illustrates the Additional Certification Info page in Partner Center.":::

---

>[!TIP]
> Do not include an email address of a company employee who can provide sign-in information. Our reviewers will **not be able to email you for sign-in information**. Applications that do not list clear instructions in the certification notes will automatically fail the submission process.

## Step 10: Use the following checklist to avoid the top five common errors that produce 80% of review rejections

Use our **[pre-submission checklist](./checklist.md)** to address all the things on this list.

- Did you include Terms of Use links?
- Did you include Privacy Policy links?
- Did you including Testing instructions for the Reviewer?
- Did you indicate Service or Account disclosures?
- Did you indicate any Additional Charge Disclosures for required paid services?

Once you have answered those questions for yourself, select the submit button on your app for review and approval.

## Step 11: Congratulations, you are done submitting!

Expect a response within 3 to 4 business days from our reviewers if there are any issues related to your submission.
