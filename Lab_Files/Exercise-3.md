# Exercise 03: Integrating Eventhouse with Lakehouse

### Estimated duration: 60 Minutes

## 📘 Scenario

To provide additional business context for the streaming events, the AdventureWorks Data Engineering team creates a **Lakehouse** to store product reference information. They then connect the Lakehouse and Eventhouse using shortcuts and build KQL tables and transformation logic that convert raw event data into structured datasets suitable for analysis and reporting.

## 📖 Overview

In this exercise, you create a **Lakehouse**, upload product reference files, generate Delta tables, create shortcuts between the Lakehouse and Eventhouse, and build the KQL schema by creating tables, functions, and update policies that transform raw streaming events into structured analytical data.

## 🎯 Objectives: 

In this exercise, you will be able to complete the following tasks:

- Task 1: Setting up the Lakehouse
- Task 2: Create delta tables in the lakehouse
- Task 3: Accessing Eventhouse data from the lakehouse 
- Task 4: Build the KQL DB schema

## Task 1: Setting up the Lakehouse

In this task, you will set up the Lakehouse to contain additional information for our use case and make the data from the KQL Database accessible through it.

1. To create a **Lakehouse**, first return to your assigned workspace **RTI_<inject key="DeploymentID" enableCopy="false"></inject> (1)** by clicking on it from the left navigation pane and select it **(2)**. 

    ![](media/new/E3T1S1-1802.png)

1. Click on the button **+ New Item (1)** in the toolbar and in the pop-up window, search for **Lakehouse (2)** and click on the tile **Lakehouse (3)**.

    ![](media/new/E3T1S2-1606.png)

1. In the dialog **New lakehouse**, enter the following details:

    - Name: **WebSalesData_LH** **(1)**
    - Location: **RTI_<inject key="DeploymentID" enableCopy="false"></inject>** **(2)**
    - Lakehouse schema: **Unchecked (3)**

        > **Note:** Make sure the Lakehouse schema is unchecked. Keeping it checked may cause issues in the later steps of the exercise.
    - Click on **Create (4)**

        ![](media/E3T1S3-1802.png)

1. You will be navigated to the overview page of the new lakehouse.

    ![](media/E3T1S4-1802.png)

> **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
> - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task. 
> - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
> - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help you out.

<validation step="36131701-01b8-4da8-bb9d-b1a686ec3df5" />

## Task 2: Create delta tables in the lakehouse

After our lakehouse has been created, the overview page of the lakehouse will be displayed. In this task, we will load static data into our new lakehouse. To do so please execute the following steps.

1. Click on the button **Get data (1)** in the toolbar and select **Upload files (2)** from the dropdown menu.

    ![](media/new/27.png)

1. In the **Upload** window that opens on the right side, click on the **Browse option (1)**. Navigate to `C:\LabFiles` **(2)** and select the two files **products.csv** and **productcategory.csv** **(3)**. Then click on the button **Open (4)**.

    ![](media/E3T2S2-1802.png)

    >**Note:** To select the two files at once, you can just hold the CTRL key while you click the two files.

1. Click on the **Upload** button. Now the files will be uploaded. Close the Upload files window.

    ![](media/E3T2S3-1802.png)

1. To verify that the files have been uploaded successfully, click on the folder **Files** in the **Explorer** pane. You should see the files in the list **Files** in the right part of the window.

    ![](media/image_task10_step04up2.png)

1. Next, we have to create delta tables in our Lakehouse from the files we uploaded. 

1. Hover over the productscategory.csv file. Click the **ellipsis (…) (1)**, choose **Load to Tables (2)** from the context menu, and then select **New table (3)** from the submenu.

    ![](media/20042026(3).png)

1. Retain all default values and click on the button **Load**.

    ![](media/image_task10_step06up2.png)

1. Repeat the same steps for the **products.csv** file to create a Delta table. By the end of this step, you should have two Delta tables in your lakehouse, created from the two uploaded CSV files.

1. Your lakehouse should look like this:

    ![](media/20042026(4).png)

## Task 3: Accessing Eventhouse data from the lakehouse

In this task, you will make the Eventhouse tables from the KQL Database available in our Lakehouse. This will be accomplished by creating shortcuts.

1. Click on the button **Get data (1)** in the menu bar at the top. Choose **New table shortcut (2)** from the dropdown menu.

    ![](media/E3T3S1.png)

    >**Note:** If your Lakehouse is using Schemas, you will see the schema db under the folder Tables. Right-click the schema dbo and select the option New table shortcut from the context menu.

1. Select **Microsoft OneLake**.

    ![](media/guide-35up2.png)

1. Select the KQL Database **WebEvents_EH (1)** and click on **Next (2)**.

    ![](media/new/17.png)

1. In the New shortcut window, expand the folder **Tables** under **WebEvents_EH** and check both tables **BronzeClicks** and **BronzeImpressions (1)**. Click on **Next (2)**.

    ![](media/image_task11_step04up2.png)

1. Click on **Create** button.

    ![](media/E3T3S6-1802.png)

1. Now you can see the shortcuts to the tables **BronzeClicks** and **BronzeImpressions** under the folder **Tables** in the lakehouse **WebSalesData_LH**.

    ![](media/20042026(5).png)

## Task 4: Build the KQL DB schema

In this task, you will create all the silver tables, functions, and enable update policies in our Eventhouse KQL Database. Two of the tables (`product` and `productCategory`) are shortcuts to the lakehouse, and the data is not being copied into our KQL Database.

1. From the left navigation pane, select your workspace **RTI_<inject key="DeploymentID" enableCopy="false"></inject>** **(1)**,  then select the KQL Database **WebEvents_EH (2)**.

    ![](media/new/18.png)

1. By now, data has already streamed into your KQL database. You can see this by looking at the dashboard that is provided on the overview page of the KQL-Database    

    ![](media/new/20.png)

1. Select the **WebEvents_EH (1)** database. Click on the button **+ New (2)** in the top toolbar and choose **OneLake shortcut (3)** from the drop-down menu.

    ![](media/new/E3T4S3a-1606.png)

    ![](media/new/E3T4S3b-1606.png)

1. Select **Microsoft OneLake**.

    ![](media/guide-35up342.png)

1. Select the Lakehouse **WebSalesData_LH (1)** and click on the button **Next (2)**.

    ![](media/guide-39up2.png)

1. Expand the Tables folder, and select the **products** and **productcategory** **(1)** tables. Click the **Next** **(2)** button, then click **Create (3)** on the next page. This will create shortcuts to the **products** and **productcategory** tables in your Lakehouse without copying the data from the Lakehouse to the Eventhouse.

    ![](media/guide-40up2.png)

    ![](media/new/E3T4S6.2-1802.png)

1. On the **Shortcut creation completed** pop up, click **Close**.

    ![](media/new/24.png)

1. Expand the **Shortcuts** folder in the tree of your KQL database **WebEvents_EH** to verify if the 2 shortcuts have been created correctly.

    ![](media/new/25.png)

1. Click on the button **Query with code** at the top of the screen.

    ![](media/new/26.png)

1. The pop-up window **Query with code** will be shown.

    ![](media/guide-44up5542.png)

1. Remove the existing code, then copy the code below and paste it into the Queryset. After that, click **Run**.

    ```kusto
    .execute database script <|
    //SILVER LAYER
    .create table SilverClicks (
        eventType:string, 
        eventID:string, 
        eventDate:datetime, 
        productId:long, 
        userAgent:dynamic, 
        device:string, 
        ip_address:string, 
        referer:dynamic, 
        page_loading_seconds:real, 
        clickType:string, 
        clickPathTitle:string, 
        clickPathUrl:string
    )
    //
    .create table SilverImpressions (
        eventType:string, 
        eventID:string, 
        eventDate:datetime, 
        productId:long, 
        userAgent:dynamic, 
        device:string, 
        ip_address:string, 
        page_loading_seconds:real, 
        relatedProductCategory:string, 
        relatedProductId:string, 
        relatedProductName:string
    )
    // use update policies to transform data during Ingestion
    .create-or-alter function with (folder="Bronze to Silver Transformations") expandClickpath()
    {
    BronzeClicks
    | mv-expand extraPayload
    | evaluate bag_unpack(extraPayload)
    | project 
        eventType, 
        eventID, 
        todatetime(eventDate), 
        productId, 
        userAgent, 
        device, 
        ip_address, 
        referer, 
        toreal(page_loading_seconds), 
        clickType = clickType, 
        clickPathTitle = ['title'], 
        clickPathUrl = url
    }
    //
    .alter table SilverClicks policy update @'[{"Source": "BronzeClicks", "Query": "expandClickpath", "IsEnabled" : true, "IsTransactional": false }]'
    //
    .create-or-alter function with (folder="Bronze to Silver Transformations") expandRelatedProducts()
    {
    BronzeImpressions
    | mv-expand extraPayload
    | evaluate bag_unpack(extraPayload)
    | project 
        eventType, 
        eventID, 
        todatetime(eventDate), 
        productId, 
        userAgent, 
        device, 
        ip_address, 
        toreal(page_loading_seconds), 
        relatedProductCategory, 
        relatedProductId, 
        relatedProductName
    }
    //
    .alter table SilverImpressions policy update @'[{"Source": "BronzeImpressions", "Query": "expandRelatedProducts", "IsEnabled" : true, "IsTransactional": false }]'
    //
    .create-or-alter function with (docstring = "Social Media Campaign Clickstream", folder = "Gold Views") SocialMediaCampaignClickstream()
    {
    SilverClicks
    | extend CampaignType = tostring(referer.campaignType)
    | extend Platform = tostring(userAgent.platform)
    | extend Browser = tostring(userAgent.browser)
    | extend RefererUrl = tostring(referer.url)
    | extend AdTitle = tostring(referer.adTitle)
    | where CampaignType in ("facebook", "twitter", "instagram", "pinterest")
    | project-away userAgent, referer
    | project-reorder CampaignType
    }
    //
    .create-or-alter function with (docstring = "Search Media Campaign Clickstream", folder = "Gold Views") SearchMediaCampaignClickstream()
    {
    SilverClicks
    | extend CampaignType = tostring(referer.campaignType)
    | extend Platform = tostring(userAgent.platform)
    | extend Browser = tostring(userAgent.browser)
    | extend RefererUrl = tostring(referer.url)
    | extend AdTitle = tostring(referer.adTitle)
    | where CampaignType in ("google", "bing")
    | project-away userAgent, referer
    | project-reorder CampaignType
    }
    //
    .create-or-alter function with (docstring = "Email Campaign Clickstream", folder = "Gold Views") EmailCampaignClickstream()
    {
    SilverClicks
    | extend CampaignType = tostring(referer.campaignType)
    | extend Platform = tostring(userAgent.platform)
    | extend Browser = tostring(userAgent.browser)
    | extend RefererUrl = tostring(referer.url)
    | extend EmailId = tostring(referer.emailId)
    | where CampaignType in ("email")
    | project-away userAgent, referer
    | project-reorder CampaignType
    }
    ```

    ![](media/guide-44up23156.png)

1. The above executed script sets up the analytics layer for the lab by creating processed (Silver) tables, transformation functions, and automatic update policies. This ensures that incoming streaming events are converted into structured, query-ready data that will be used in the Real-Time Dashboard and later analysis.

1. The status of the execution of the commands from the file **createAll.kql** can be seen at the bottom of the pane. The result of each Command should be **Completed**.

    ![](media/image_task12_step09bup2.png)

1. Click on the pencil icon beside **Tab** and rename the tab to **createAll**.

    ![](media/guide-45up2.png)

1. Expand all folders in the database pane on the left. All tables and functions that have been created by the script can be found here.

    ![](media/guide-46up2.png)

## 🧾 Summary

In this exercise, you created a Lakehouse, loaded reference CSV files as Delta tables, established shortcuts between the Lakehouse and Eventhouse, and built the KQL schema consisting of Silver tables, transformation functions, and update policies. These steps prepare the streaming data for efficient querying and visualization.

### You have successfully completed the exercise. Now, click on **Next >>** from the lower right corner to proceed to the next exercise.

![](media/new/next.png)
