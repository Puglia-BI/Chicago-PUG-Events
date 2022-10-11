
# Dataflows for Enterprise - Outline

## Introduction

* Dataflows used today (standard loads)
* The ETL Process & The needs
* Why Dataflows as a solution

## Dataflows As a Primer (Basic Features)

* What Can they do?
* Connect To same datasources as BI
* Query Editor is Incredible
* Loads to Power BI Dataset

## Why Use Dataflows (Solution Basic)

* Reuse Queries / Tables in multiple reports
* Create Master Reference Tables for BI Teams
* No need to recreate the Wheel 
* Less load on PBI Dataset

## Bigger Solution & Better Use Cases

* More Complexity is needed
* More Datasources to connect too
* Reuse Staging Queries for Other Transformation Needs
* Works with Datamarts!
* Performance Incredible Increase using Computed / Linked Entities

## Dataflows as Data Pound - The Basics

* Using Dataflows as a Data Pound
  * Bronze, Silver Gold
  * Promote Master / Certified Queries 
* Performance increase
* Sync with Azure Data Lake
* Create an ETL process in your team and org
  * https://learn.microsoft.com/en-us/power-bi/guidance/powerbi-implementation-planning-usage-scenario-advanced-data-preparation
* Promote Self-Service to Data Pond easily 

## Data Pond - The Benefits

* Computed Entities
* Linked Entities
* Incremental Refresh
* Enhanced Computed Engine
* Datamart Integration

## Data Pond Structure

* Dev Workspace
  * For Staging / Transformations
  * Think of Structure (Tenant & Access)
* Staging Dataflows (Bronze)
  * Datasources are separated into different dataflows
  * Minimal Transformations
* Transformation Dataflows (Silver)
  * Located in same workspace as Bronze (source)
  * Connect to Bronze tables
  * Reference, and then transform 
    * Not on Source Query!
* Prod Workspace
  * Where users / teams will pull in Power BI datasets the dataflow
  * Think of access to this workspace 
* Final Dataflow (Gold)
  * Reference to Silver entities
  * Organize based on purpose

## Data Pond Process



[//]: # (Concepts)

# Important Concepts

### Advanced Data Prep

### Computed Entities

#### Notes
* In-storage computations when using dataflows. Do Calculations on existing dataflows. 
* Calculations do not run against external data source
* 

####  Computed Entities Steps

1. Load Dataflow
2. Connect to Dataflow form another Dataflow
3. Reference The query
4. 

![](https://learn.microsoft.com/en-us/power-query/dataflows/media/dataflows-computed-entities/computed-entities-premium-00.png)

### Linked Entities

#### Notes

* References a table from another dataflow. Does not duplicate the data, it ruses the standard table multiple times.
* Used in the transformation dataflow for accessing from staging
* Difference between workspaces
  * In two different workspaces: Refresh to link from source workspace behaves like a link to an external data source. Does not automatically affect the data in destination dataflow
  * Same Workspace: Refresh occurs in source dataflow, that event auto triggers a refresh for dependent entities in all destination dataflows. All other entities in destination dataflow are refreshed according to dataflow schedule
  * A linked entity can't be joined with a regular entity that gets its data from an on-premises data source.
  * When using M parameters to address linked entities, if the source dataflow is refreshed, it doesn't automatically affect the data in the destination dataflow.

#### Linked Steps


### Enhanced Computed Engine

#### Notes

* Improves performance of linked tables within the same workspace
* Use the same workspace bronze / silver
* Use Query Folding when possible

### Incremental Refresh

### Store Data in DDataflow Storage

# Steps for Demo

## Bronze Dataflows

1. Creating Bronze Dataflows
2. From SQL (NorthWind) All Tables - brz_sql_NorthWind
3. From SharePoint - CSV & Excel Files - brz_sharepoint_VanArsdel
   1. Think of using that format for dataflows - [level_datasource_name]
4. Load and Refresh

## Silver Dataflows - Basics

1. In same workspace, create new dataflow, connect to linked entities 
2. Choose only sql one first
3. Add to Group
4. Notice the linked entity symbol
5. Add dataflow from SharePoint
6. add to group
7. Showcase what happens when trying to edit linked entity

## Silver dataflows - Transform

### CSVSales

1. Reference SalesCSV
2. Rename stgSalesCSV
3. Add to new Group - silver
4. Append all countries
5. Merge Zip & Country
6. Rename Column to ShippedGeo

### Geo

1. Reference GeoDim
2. Rename stgGeoDim
3. Merge Zip & Country as new Column - ShippedCountry
4. Reference GeoSheet
5. Rename stgGeoSheet
6. Do same as 3
7. Append as New
8. stgGeoSheet & stgGeoDim
9. Remove Duplicates
10. Rename slvGeoMaster
11. Add to Gold Folder

### Orders

1. Reference Orders & Order Details
2. Change Dates to Date Time in Orders
3. Filter Rows for only through today
   1. [OrderDate] = Date.From(DateTime.LocalNow()) or [OrderDate] < Date.From(DateTime.LocalNow()))
4. Merge Customers and get Postal Code & Country
5. Rename stgOrders
6. Rename stgOrderDetails
7. Merge Orders together
8. Expand Zip & Country, Customer ID, Order Date
9. Merge both postal code & country dates

### Final Sales

* Append stgCSVSales & stgOrderDetails

