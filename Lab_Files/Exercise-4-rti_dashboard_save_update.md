# Exercise 04: Building an Interactive Real-Time Dashboard with Live Data

### Estimated duration: 90 Minutes

## 📘 Scenario

After the streaming pipeline and data model are complete, the operations team at AdventureWorks needs a **live monitoring solution**. You create a **Real-Time Dashboard** that visualizes incoming event data, configure automatic refresh so the dashboard stays current, and enable a Data Activator rule that can generate an automated response when a specified condition is met.

## 📖 Overview

In this exercise, you create a **Real-Time Dashboard** that queries the **processed event data** stored in the Eventhouse, build multiple live visualizations, enable continuous auto-refresh, and configure a **Data Activator** rule based on dashboard metrics to demonstrate event-driven automation.

## 🎯 Objectives:

In this exercise, you will be able to complete the following tasks:

- Task 1: Real-Time Dashboard
- Task 2: Enable Auto-refresh on your dashboard  
- Task 3: Enable Data Activators

## Task 1: Real-Time Dashboard 

In this task, you will create a Real-Time Dashboard that visualizes the processed streaming data stored in your Eventhouse KQL Database. Using KQL queries, you will build multiple dashboard tiles to monitor metrics such as clicks, impressions, geographic distribution, page load performance, and anomalies. You will also configure the dashboard for continuous refresh in a later task so that it reflects incoming streaming events in near real time.

> **Note:** For reference or an accelerated setup, a pre-built version of the dashboard is available for download [here](https://github.com/microsoft/FabricRTIWorkshop/blob/main/dashboards/RTA%20dashboard/dashboard-RTA%20Dashboard.json). The downloaded JSON file can be imported into your Microsoft Fabric workspace as a Real-Time Dashboard.

> ![](media/RealTimeDashboard.png)

> To obtain the dashboard, open the GitHub link above and click **Raw** (or **Download raw file**) to save the `.json` file locally. You can then import the downloaded JSON into Microsoft Fabric and configure it to use your own KQL Database as the data source.

> For this lab, however, you will build the dashboard manually to understand how each visualization, query, and configuration is created. This hands-on approach provides a better understanding of Real-Time Dashboard development in Microsoft Fabric.


1. From the left navigation pane, select your workspace **RTI_<inject key="DeploymentID" enableCopy="false"></inject> (1)** and select **RTI_<inject key="DeploymentID" enableCopy="false"></inject> (2)**.

    ![](media/new/5.png)

1. From the **RTI_<inject key="DeploymentID" enableCopy="false"></inject>** workspace, click on **WebEvents_EH** KQL Database to open it.

    ![](media/new/E4T1S2-1902.png)

1. To create a Real-Time Dashboard, click on the **Real-Time Dashboard** icon from the top menu bar.

    ![](media/new/E4T1S3-1902.png)

1. Enter the following details: 

    - Name - **Web Events Dashboard (1)**.
    - Location - **RTI_<inject key="DeploymentID" enableCopy="false"></inject> (2)** 
    - Click on **Create (3)**.

        ![](media/new/30.png)

1. Now, as we have created the dashboard from the KQL Database, it has automatically selected the Datasource as the KQL Database itself. 

1. If a table is already present on the dashboard, click the **ellipsis (…) (1)** at the top-right corner of the table visual and select **Delete (2)** to remove it. We will add our own visuals based on the queries we create.

     ![](media/20042026(6).jpg)

The **Real-Time Dashboard** is currently empty. In the next series of subtasks, you will gradually build the dashboard by creating individual visuals for different business metrics. Each visual highlights a specific aspect of the streaming data and contributes to the final interactive dashboard

### Task 1.1: Create the "Click by Hour" visual

1. From the **Home (1)** tab, click on the button **Add visual (2)** dropdown and select **Area chart (3)** to create a new tile for the dashboard.

    ![](media/new/E4T1.1S1-1606.png)

1. On the Query editor, paste the following query **(1)**. Set the time range as **Last 7 days (2)** and click on **Run (3)** to execute the query. 

    ```kusto
    SilverClicks
    | where eventDate between (_startTime.._endTime)
    | summarize date_count = count() by bin(eventDate, 1h)
    | render timechart
    | top 30 by date_count
    ```

    ![](media/new/E4T1.1S2-1606.png)

1. From the left navigation tab, click on the **Visualization (1)** tab. Expand the General section **(2)**, and enter the Tile name as **Clicks by Hour (3)**. Then click on **Done (4)**.

    ![](media/new/E4T1.1S3-1606.png)

1. You will be navigated to the Home page. Set the Time range as **Last 7 days**.

    ![](media/new/E4T1.1S4-1606.png)

### Task 1.2: Create "Impressions by Hour" visual

1. From the **Home (1)** tab, click on the button **Add visual (2)** dropdown and select **Area chart (3)** to create a new tile for the dashboard.

    ![](media/new/E4T1.1S1-1606.png)

1. On the Query editor, paste the following query **(1)** and click on **Run (2)** to execute the query. Then navigate to the **Visualization (3)** tab.

    ```kusto
    //Impressions by hour
    SilverImpressions
    | where eventDate between (_startTime.._endTime)
    | summarize date_count = count() by bin(eventDate, 1h)
    | render timechart
    | top 30 by date_count
    ```

    ![](media/new/E4T1.2S2-1606.png)

1. Expand the General section, and enter the Tile name as **Impressions by Hour (1)**. Then click on **Done (2)**.

    ![](media/new/E4T1.2S3-1606.png)

### Task 1.3: Create "Impressions by Location" visual.

1. From the **Home (1)** tab, click on the button **Add visual (2)** dropdown and select **Map (3)** to create a new tile for the dashboard.

    ![](media/new/E4T1.3S1-1606.png)

1. On the Query editor, paste the following query **(1)** and click on **Run (2)** to execute the query. Then navigate to the **Visualization (3)** tab.

    ```kusto
    //Impressions by location
    SilverImpressions
    | where eventDate  between (_startTime.._endTime)
    | join external_table('products') on $left.productId == $right.ProductID
    | project lon = toreal(geo_info_from_ip_address(ip_address).longitude), lat = toreal(geo_info_from_ip_address(ip_address).latitude), Name
    | render scatterchart with (kind = map) //, xcolumn=lon, ycolumns=lat)
    ```

    ![](media/new/E4T1.3S2-1606.png)

1. Expand the General section, and enter the Tile name as **Impressions by Location (1)**. Then click on **Done (2)**.

    ![](media/new/E4T1.3S3-1606.png)

### Task 1.4: Create the "Average Page Load Time" visual

1. From the **Home (1)** tab, click on the button **Add visual (2)** dropdown and select **Time chart (3)** to create a new tile for the dashboard.

    ![](media/new/E4T1.4S1-1606.png)

1. On the Query editor, paste the following query **(1)** and click on **Run (2)** to execute the query. Then navigate to the **Visualization (3)** tab.

    ```kusto
    //Average Page Load time
    SilverImpressions
    | where eventDate   between (_startTime.._endTime)
    //| summarize average_loadtime = avg(page_loading_seconds) by bin(eventDate, 1h)
    | make-series average_loadtime = avg(page_loading_seconds) on eventDate from _startTime to _endTime+4h step 1h
    | extend forecast = series_decompose_forecast(average_loadtime, 4)
    | render timechart
    ```

    ![](media/new/E4T1.4S2-1606.png)

1. Expand the General section, and enter the Tile name as **Average Page Load Time (1)**. Then click on **Done (2)**.

    ![](media/new/E4T1.4S3-1606.png)

### Task 1.5: Create KPI Summary tiles

1. From the **Home (1)** tab, click on the button **Add visual (2)** dropdown and select **Time chart (3)** to create a new tile for the dashboard.

    ![](media/new/E4T1.5S1-1606.png)

1. On the Query editor, paste the following query **(1)** and click on **Run (2)** to execute the query. Then navigate to the **Visualization (3)** tab.

    ```kusto
    //Clicks, Impressions, CTR
    let imp =  SilverImpressions
    | where eventDate  between (_startTime.._endTime)
    | extend dateOnly = substring(todatetime(eventDate).tostring(), 0, 10)
    | summarize imp_count = count() by dateOnly;
    let clck = SilverClicks
    | where eventDate  between (_startTime.._endTime)
    | extend dateOnly = substring(todatetime(eventDate).tostring(), 0, 10)
    | summarize clck_count = count() by dateOnly;
    imp
    | join clck on $left.dateOnly == $right.dateOnly
    | project selected_date = dateOnly , impressions = imp_count , clicks = clck_count, CTR = clck_count * 100 / imp_count
    ```

    ![](media/new/E4T1.5S2-1606.png)

1. Expand the General section, and enter the Tile name as **Impressions (1)**. From the Data section, in the Value column select **impressions (long) (2)**. Then click on **Done (3)**.

    ![](media/new/E4T1.5S3-1606.png)

1. Click the **ellipsis (...)** **(1)** at the top right of the Impressions tile you just created and select **Duplicate (2)** and duplicate it two times.

    ![](media/new/E4T1.5S4-1606.png)

    ![](media/new/E4T1.5S5-1606.png)

1. Click the **ellipsis (...)** **(1)** on 1st duplicate and select **Edit (2)** to edit the tile.

    ![](media/new/E4T1.5S6-1606.png)

1. From the Visualization tab, enter the **Tile name** as **Clicks (1)**, set the Data value column to **clicks (long) (2)**, then click on **Done (3)**.

    ![](media/new/E4T1.5S7-1606.png)

1. Click the **ellipsis (...)** on another duplicate and select **Edit** to edit the tile.

1. Enter the **Tile name** as **Click Through Rate (1)**, set the Data value column to **CTR (long) (2)**, then click on **Done (3)**.

    ![](media/new/E4T1.5S8-1606.png)

1. All three Stat visuals would look like this:

    ![](media/new/E4T1.5S10-1606.png)

### Task 1.6: Create the "Average Page Load Time Anomalies" visual

1. From the **Home (1)** tab, click on the button **Add visual (2)** dropdown and select **Anomaly chart (3)** to create a new tile for the dashboard.

    ![](media/new/E4T1.6S1-1606.png)

1. On the Query editor, paste the following query **(1)** and click on **Run (2)** to execute the query. Then navigate to the **Visualization (3)** tab.

    ```kusto
    //Avg Page Load Time Anomalies
    SilverImpressions
    | where eventDate   between (_startTime.._endTime)
    | make-series average_loadtime = avg(page_loading_seconds) on eventDate from _startTime to _endTime+4h step 1h
    | extend anomalies = series_decompose_anomalies(average_loadtime)
    | render anomalychart
    ```

   ![](media/new/E4T1.6S2-1606.png)

1. Expand the General section, and enter the Tile name as **Average Page Load Time Anomalies (1)**. Then click on **Done (3)**.

   ![](media/new/E4T1.6S3-1606.png)

### Task 1.7: Create the "Strong Anomalies" visual

1. From the **Home (1)** tab, click on the button **Add visual (2)** dropdown and select **Table (3)** to create a new tile for the dashboard.

    ![](media/new/E4T1.7S1-1606.png)

1. On the Query editor, paste the following query **(1)** and click on **Run (2)** to execute the query. Then navigate to the **Visualization (3)** tab.

    ```kusto
    //Strong Anomalies
    SilverImpressions
    | where eventDate between (_startTime.._endTime)
    | make-series average_loadtime = avg(page_loading_seconds) on eventDate from _startTime to _endTime+4h step 1h
    | extend anomalies = series_decompose_anomalies(average_loadtime,2.5)
    | mv-expand eventDate, average_loadtime, anomalies
    | where anomalies <> 1
    | project-away anomalies
    ```

    ![](media/new/E4T1.7S2-1606.png)

1. Expand the General section, and enter the Tile name as **Strong Anomalies (1)**. Then click on **Done (3)**.

   ![](media/new/E4T1.7S3-1606.png)

### Task 1.8: Add the dashboard logo and arrange the layout

1. To add a **Logo**, click on the **Add Markdown** button from the top menu bar.

    ![](media/new/E4T1.8S1-1606.png)

1. Paste **(1)** the following code in the text area. Give the tile name as **Logo (2)** and click on **Done (3)**.

    ```kusto
    ![AdventureWorks](https://raw.githubusercontent.com/CloudLabsAI-Azure/FabConRTITutorial/main/Lab_Files/media/AdventureWorksLogo.png "AdventureWorks")
    ```

   ![](media/new/E4T1.8S2-1606.png)

   >**Note:** The Tile can be resized on the dashboard canvas directly, rather than writing code.

1. After you have added all the visuals and moved them to their appropriate places, your dashboard should look similar to the image below.

    ![](media/new/E4T1S39-1802.png)

> **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
> - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task. 
> - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
> - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help you out.

<validation step="02a20e12-54b5-4b37-9c8f-d4198f9f4430" />

## Task 2: Enable auto-refresh on your dashboard

In this task, you will enable auto-refresh so the dashboard will be automatically updated while it is shown on screen.

1. Click on the **Manage (1)** tab and then click on the button **Refresh settings (2)**. This will open a pane on the right side of the browser.

    ![](media/new/E4T2S1-1606.png)

1. In the **Refresh settings** window, which opens on the right side, select **Live refresh (1)** then click on **Apply (2)** button. You can click on the Settings button under Live refresh to view or change the refresh settings. 

    ![](media/new/E4T2S2-1606.png)

1. Click on the **Home (1)** tab and then click on the **Save (2)** button.

    ![](media/new/52.png)

## Task 3: Enable Data Activators

In this task, you will create a Reflex Alert that will send a Teams Message when a value meets a certain threshold.

1. Click on the **ellipsis** **(...) (1)** of the tile **Click by hour**. Select **Add alert (2)** from the context menu. This will open the **Add Rule** pane on the right side in the browser.

    ![](media/new/E4T3S1-1606.png)

1. Enter the name as **Click Alert**

    ![](media/new/E4T3S2-1606.png)

1. In the **Condition** section, fill the fields with the following values:

    | Field                | Value                      |
    |----------------------|----------------------------|
    | Check **(1)**               | **On each event when**  |
    | Grouping field **(2)**      | **eventDate**                |
    | When **(3)**                 | **date_count**                |
    | Condition **(4)**            | **Number State- is greater than**      |
    | Value **(5)**                | **250**                     |
    | Occurance **(6)**           | **Every time the condition is met**           |

    ![](media/new/E4T3S3-1606.png)

1. In the Actions section, choose **Email (1)** as the action type.

1. Select the workspace as **RTI_<inject key="DeploymentID" enableCopy="false"></inject> (2)**. For Item, choose **Create a new item (3)**. Enter **Activator (4)** in the New item name field, and then click **Create (5)**.
    
   ![](media/new/E4T3S4-1606.png)

1. The Reflex item will appear in your workspace, and you can edit the Reflex trigger action. The same Reflex item can also trigger multiple actions.

## 🧾 Summary

In this exercise, you have built a real-time dashboard in Microsoft Fabric to visualize streaming data. You created various visuals such as time charts, maps, anomaly charts, and tables to gain insights from the data. Additionally, you enabled auto-refresh to ensure that the dashboard updates in real-time as new data arrives. Finally, you set up a Data Activator to automate actions based on specific conditions in your data, allowing for proactive responses to important events.

### 🎉 You have successfully completed the Build a Fabric Real-Time Intelligence Solution in a Day lab.

During this lab, you created a Fabric Workspace and Eventhouse, enabled OneLake Availability, configured an Eventstream, and used a notebook to generate synthetic streaming web events. You then routed click and impression events into Eventhouse tables, created a Lakehouse with reference data, established shortcuts between storage layers, and built a KQL schema with transformation functions and update policies.

Finally, you developed a Real-Time Dashboard to visualize live streaming data, enabled automatic refresh for continuous monitoring, and configured a Data Activator rule to demonstrate automated responses based on incoming events.

The completed solution illustrates how Microsoft Fabric can be used to build an end-to-end real-time analytics pipeline that ingests, transforms, stores, visualizes, and monitors streaming event data while integrating seamlessly across Eventhouse, OneLake, Lakehouse, KQL, dashboards, and automation features.
