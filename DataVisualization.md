# Data Visualization using Tableau
This setup will allow you to create a Tableau dashboard that visualizes live temperature data, with continuous updates pulled directly from PostgreSQL.

To display live temperature data from PostgreSQL in Tableau:

- [/] Install PostgreSQL ODBC driver (if not already installed).
- [/] Connect Tableau to PostgreSQL by providing server, database, and authentication details.
- [/] Set the connection to Live (rather than extract) to ensure real-time data fetching.
- [/] Create visualizations (like time series charts) to represent temperature data over time.
- [/] Build a dashboard by arranging the visualizations on the canvas.
- [/] Optionally, set up auto-refresh to keep the dashboard updated at regular intervals.
- [/] Publish the dashboard to Tableau Server or Tableau Online if sharing with others.

# Tableau Setup to Display Live Temperature Data from PostgreSQL

This guide explains how to connect Tableau to a PostgreSQL database, visualize temperature data, and create a live-updating dashboard.

## Prerequisites

- **PostgreSQL Database**: A PostgreSQL database containing temperature data.
- **Tableau Desktop**: Installed on your machine.
- **PostgreSQL ODBC Driver**: Installed on your machine to connect Tableau to PostgreSQL.
- **Python Kafka Producer**: A producer script that sends live temperature data to PostgreSQL (already set up).

## Step 1: Install PostgreSQL Database Driver

Before connecting Tableau to PostgreSQL, ensure you have the correct PostgreSQL ODBC driver installed.

1. **Download the PostgreSQL ODBC Driver**:
   - Visit the [PostgreSQL ODBC driver page](https://www.postgresql.org/ftp/odbc/versions/msi/) and download the appropriate version for your operating system.
   - Install the driver on your machine.

## Step 2: Open Tableau and Connect to PostgreSQL

1. **Launch Tableau Desktop**.
2. **Connect to PostgreSQL**:
   - In the **Connect** pane on the left, click **PostgreSQL** under the **"To a Server"** section.
   - Enter the following details in the **PostgreSQL Connection** dialog:
     - **Server**: The IP address or hostname of your PostgreSQL server (e.g., `localhost`).
     - **Port**: The default PostgreSQL port (`5432`).
     - **Database**: The name of your PostgreSQL database (e.g., `temperature_db`).
     - **Authentication**: Provide the **username** and **password** for your PostgreSQL database.
   - Click **"Sign In"** to establish the connection.

## Step 3: Select the Temperature Data Table

1. Once connected, Tableau will show the list of schemas and tables.
2. **Select the `temperature_data` table** (or whichever table contains your temperature data).
3. **Drag the table to the canvas** to load the data into Tableau.

## Step 4: Configure Live Data Connection

1. **Set Connection to Live**:
   - Tableau will default to an **Extract** connection type.
   - To use live data, click on the **"Live"** option in the top-right corner of Tableau to ensure real-time data fetching from PostgreSQL.
2. **Test the Live Connection**:
   - Drag fields such as `timestamp` and `temperature` from the **Data Pane** to the **Columns** and **Rows** shelves.
   - Verify that data is being pulled in real time from PostgreSQL.

## Step 5: Create Your Visualization

1. **Create a Worksheet**:
   - Go to the **Worksheet** tab at the bottom to start creating your visualization.
   - Drag the `timestamp` field to the **Columns** shelf.
   - Drag the `temperature` field to the **Rows** shelf.
   - Tableau will generate a **line chart** or **scatter plot** by default.
   - Customize the chart by adjusting the axis, adding labels, and formatting the chart as needed.
   
2. **Customize the Time Field**:
   - You can format the `timestamp` field by right-clicking it in the **Columns** shelf and selecting **Format** to display it in a readable format.
   - Change the aggregation of the `timestamp` field to adjust the time intervals (e.g., by minute, hour, etc.).

3. **Additional Visualizations (Optional)**:
   - You can create more visualizations (e.g., average temperature per hour) by dragging and dropping additional fields.
   - Use **filters** or **calculated fields** to enhance your analysis.

## Step 6: Build the Dashboard

1. **Create the Dashboard**:
   - Once the worksheet(s) are ready, go to the **Dashboard** tab to start building the dashboard.
   - Drag the created worksheet(s) onto the dashboard canvas.
   - Arrange the visualizations as needed to create a cohesive dashboard view.
   - Resize and format the elements to suit your design.

## Step 7: Set Auto-Refresh for Live Data (Optional)

1. **Enable Auto-Refresh**:
   - To ensure the data is continuously updated, configure Tableau to refresh the live connection automatically.
   - Go to **Data > Refresh All Extracts** in the top menu.
   
2. **Set Auto-Refresh Interval**:
   - Tableau will automatically refresh the live connection based on the data interval set.
   - For instance, if you want the data to refresh every 5 minutes, configure the **Auto-Refresh** feature.

## Step 8: Publish the Dashboard to Tableau Server (Optional)

If you wish to share the live dashboard with others, you can publish it to Tableau Server or Tableau Online.

1. **Publish the Dashboard**:
   - Click on **Server** in the top menu and select **Tableau Server** or **Tableau Online**.
   - Sign in to your Tableau Server or Tableau Online account.
   - Choose a project and click **Publish**.

2. **Access the Dashboard**:
   - Once published, the dashboard will be accessible via the Tableau Server or Tableau Online URL.
   - The live data will continue to update as new data comes in from PostgreSQL.

## Step 9: Final Testing and Monitoring

1. **Test the Live Data**:
   - Ensure that the dashboard updates in real-time as new temperature data is added to the PostgreSQL database.
   - Test by generating new temperature data via your Kafka producer and verifying the changes in the Tableau dashboard.

2. **Monitor Performance**:
   - Ensure that your PostgreSQL server is capable of handling the load from Tableau's real-time queries.
   - Optimize PostgreSQL performance and use connection pooling if necessary, especially if you have a high data volume.




