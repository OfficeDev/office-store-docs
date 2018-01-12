---
title: Add lead management details for your Office Add-ins in the Seller Dashboard
description: Use leads to get information about users who acquire your add-in, and follow up with customers directly to ensure they have a successful experience with your solution.
ms.date: 1/11/2018
---

# Add lead management details for your Office Add-ins in the Seller Dashboard

To get information about users who acquire your add-in, you can submit lead configuration details for your customer relationship management (CRM) system in the Seller Dashboard. 

You can use leads to follow up with customers directly to ensure that they have a successful experience with your solution. 

For customers who acquire your add-in via AppSource, the following details are sent to your lead management system:

- First Name
- Last Name
- Email Address

> [!NOTE]
> Leads support for acquisitions made via the AppSource and in-client experiences are coming soon.

## Add lead management information to the Seller Dashboard

As part of the submission process, you will be prompted to submit lead configuration details so that you can receive lead information. On the **Lead Management** tab, select the **I want to receive lead information from users who acquire my add-in** check box, and provide the following information.

|**Field**|**Description**|
|:-----|:-----|
|Offer Display Name|A title to annotate how the lead was generated. For example, Contoso Business Planner Add-in.|
|Publisher Display Name|A title to represent your publisher information within the lead. For example, Contoso Add-in Development team.|
|Lead Destination|Select the applicable CRM or storage service from the drop-down.|

The steps to complete your lead submission process varies based on the CRM system provider that you choose.  

> [!NOTE]
> If your CRM system provider is not listed, we recommend that you select Azure Table. Most popular CRM services can integrate with this storage service.

### Dynamics CRM Online

For Microsoft Dynamics CRM systems, you need to provide the following information:

- CRM URL
- User Name
- Password 

For information about setting up a new Dynamics service for leads, see the [Appsource Dynamics documentation](https://aka.ms/leadsettingfordynamicscrm).

> [!NOTE]
> To configure Dynamics CRM for leads, some services require the user to be an admin on your Dynamics CRM instance, and a tenant admin to create a new service account.  

### Azure Table

Select **Azure Table** to output lead information into a Microsoft Azure-hosted storage table. To get started with Azure, see [Create your free Azure account](https://azure.microsoft.com/en-us/free/).

You must submit a **Connection String** value to submit your lead management details. To find or generate this value:

 1. In the [Azure portal](https://ms.portal.azure.com/), select the storage account the lead should be sent to. To create a new storage account, select **Storage accounts** in the left navigation, and then select **Add** in the top-left corner of the header.
 
 2. Under **Settings**, select **Access Keys**.
 
 3. Copy the storage account key shown under **Primary Connection String**.
 
 4. Paste this string into the **Connection String** field in the Seller Dashboard.

### Salesforce

To direct your lead information to a Salesforce CRM system, you need to provide an **Object Identifier** value. To find this value:

 1. In your Salesforce CRM system, go to **Setup**.
 
 2. In the Quick Find search bar, enter **Web-to-Lead**.
 
 3. Select **Create Web-to-Lead Form**. 
 
 4. Ignore the form on the next page, and select **Generate**.
 
 5. In the generated form, copy the **oid value**, with the format:

	`<input type=hidden name="oid" value="00XXXXXXXXXXXXX">`

 6. Paste the value into the **Object Identifier** field in the Seller Dashboard.

### Marketo

To direct your lead information to a Marketo CRM system, you need to provide the following values:

- Form Id
- Munchkin Id
- Server Id 

To find these values:

1. Go to **Design Studio** within Marketo.

2. Select **New Form**.

3. Fill in the fields in the New Form pop-up window.

4. Select **Finish** on the Field Details form. Approve and close the form.

5. Under **Form Actions**, select **Embed Code**.

6. Within the embed code, look for the line similar to the following:

	 `<script>MktoFormsExample.loadForm("//app-ys11.marketo.com", "123-PQR-789", 1169);</script>`

7. Extract the applicable values. 

   In this example, the following are the values to extract.

   |**Parameter name**|**Example value**|
   |:-----|:-----|
   |Form Id|ys11|
   |Munchkin Id|123-PQR-789|
   |Server Id|1169|

8. Submit these values in the Seller Dashboard. 

### Azure Blob

Select **Azure Blob** to output lead information in a CSV format within an Azure-hosted blob. To get started with Azure, see [Create your free Azure account](https://azure.microsoft.com/en-us/free/).

You must submit a **Connection String** value as well as a **Container Name** value to submit your lead management details. To find or generate these values:

1. In the [Azure portal](https://ms.portal.azure.com/), select the storage account the lead should be sent to. To create a new storage account, select **Storage accounts** in the left navigation, and then select **Add** in the top-left corner of the header.

2. Under **Settings**, select **Access Keys**.

3. Copy the storage account key shown under **Primary Connection String**.

4. Paste this string in the **Connection String** field in the Seller Dashboard.

5. In the Blob Services section for the same storage account, select **Containers**.

6. Select the container that you want to send the CSV data to, or create a new container.

7. Copy the **Name** of the container.

8. Paste this string in the **Container Name** field in the Seller Dashboard.

## Submit your lead management details

After you have completed the fields in the Seller Dashboard, select **Next**. 

If you get an error message, make sure that your details are correct or try again later. 

> [!NOTE]
> If your add-in is already in AppSource, your lead management details are saved regardless of whether your submission passes validation. You don't have to resubmit lead management details unless you want to update the CRM or storage service that your leads are sent to.


## Communication guidelines

Make sure that any correspondence you send to customers includes an option to unsubscribe from future communications. 

## See also

- [Upload your package to AppSource](upload-package.md)
- [Create your AppSource listing](office-store-listing.md)
- [Decide on a pricing model for your AppSource submission](decide-on-a-pricing-model.md)
- [Create or update client IDs and secrets in the Seller Dashboard](create-or-update-client-ids-and-secrets.md)
- [AppSource submission FAQ](office-store-submission-faq.md)
- [Use the Seller Dashboard to submit your solution to AppSource](use-the-seller-dashboard-to-submit-to-the-office-store.md)
- [Make your solutions available in AppSource and within Office](submit-to-the-office-store.md)
