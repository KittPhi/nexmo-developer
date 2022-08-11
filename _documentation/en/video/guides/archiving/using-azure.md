---
title: Archiving using a Windows Azure container.
meta_title: Use a Windows Azure container with Vonage Video API archiving.
description: You can use a Windows Azure container for Vonage Video API archiving.
product: video
navigation_weight: 1
---

# Archiving using a Windows Azure container

From the [Vonage dashboard](https://identity.nexmo.com/login?icid=nexmocustomer_api-developer-adp_nexmodashbdsigin_nav) specify your S3-compliant endpoint or Windows Azure container for completed archives to be uploaded to. (For more information on Amazon S3 see [Archiving using AWS S3](/video/guides/archiving/using-s3).)"

You can create a Windows Azure account at [http://azure.microsoft.com](http://azure.microsoft.com).

You will need to set the following in your Vonage video account:

* Your Azure account name
* Your Azure account key
* The container name
* The Azure domain (optional)

You can create an Azure container (or find names of existing containers) at the [Windows Azure portal](https://portal.azure.com). (See [this Microsoft documentation](http://azure.microsoft.com/en-us/documentation/articles/storage-create-storage-account) on creating an Azure storage account.

To obtain an Account Key:

1. At the [Windows Azure portal](https://portal.azure.com), click **All Resources** on the left-hand menu.
2. Click the storage account that you want to use for archiving, and then click the **Settings** button.
3. Click **Keys** in the right-hand side of the page.
4. Make a record of the storage account name and one of the access keys (either the primary access key or the secondary access key).
5. Click the **Blobs** button and record the blob (storage container) name you will use to store archives.

Now, log in to your [Vonage Dashboard](https://identity.nexmo.com/login?icid=nexmocustomer_api-developer-adp_nexmodashbdsigin_nav) and complete the following steps:

1. Click on a **Projects** in the left nav that will contain sessions that you are archiving.
2. Click the **Project Archives** tab.
3. In the Archiving section, click the **Set up your cloud storage now** button, and then click on **Microsoft Azure**.
4. Enter the following:
    * **Account name** — The Windows Azure storage account name
    * **Account key** — The Windows Azure storage account access key
    * **Container** — The Windows Azure blob (storage container) name
    * **Domain** (optional) — The Windows Azure domain in which the container resides
5. Then click the **Connect to cloud storage** button.

<!-- **Note:** You can also set an archive upload target using the [Vonage Video REST API](/developer/rest/#setting-an-amazon-s3-or-windows-azure-archiving-upload-target). -->

<!-- OPT-TODO: Add a link to the video API reference  -->

Archives are uploaded to the Windows Azure container you specify.

All archives are saved to a subdirectory of your Azure container that has your Vonage App ID as its name, and each archive is saved to a subdirectory that has the archive ID as its name. The name of the archive file is archive.mp4 (for a composed archive) or archive.zip (for an individual stream archive). (See [Individual stream and composed archives](/video/guides/archiving/overview#individual-stream-and-composed-archives).)

For example, consider an archive with the following App ID and ID:

* App ID -- 123456
* Archive ID -- **ab0baa3d-2539-43a6-be42-b41ff1488af3**

The file for this archive is uploaded to the following directory in your Azure container:

**123456/ab0baa3d-2539-43a6-be42-b41ff1488af3/archive.mp4**