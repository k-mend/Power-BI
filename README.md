
#  Kenya Real Estate Housing Dashboard

### 1.  Data Source

The data is sourced from a **PostgreSQL** database containing comprehensive information on:

- Properties (`Houses` table)  
- Owners (`Owners` table)  
- Taxes (`Taxes` table)  
- Utilities (`Utilities` table)  

---

### 2.  Data Import Process (Power BI Desktop)

Follow these steps to import data into Power BI Desktop:

1. **Open Power BI Desktop**.
2. **Get Data**:  
   - On the "Home" tab, click **Get Data**.
   - Choose **PostgreSQL database**.
3. **Connection Settings**:  
   - Input the **server name/IP**, **database name**, and optional **port number**.
4. **Credentials**:  
   - Select **Database** authentication.  
   - Enter your **PostgreSQL username and password**.
5. **Connect**: Click **Connect**.
6. **Navigator**:  
   - Select the following tables:  
     - `Houses`  
     - `Owners`  
     - `Taxes`  
     - `Utilities`
7. Click **Transform Data** to open **Power Query Editor**.

---

### 3.  Data Transformation (Power Query Editor)

####  Houses Table
- **Title**: No changes  
- **Price**:  
  - Removed `"KSh"` prefix  
  - Converted from `Text` to `Decimal Number`  
- **PropertyID**: No changes  
- **Bedrooms**:  
  - Removed `" bedrooms"` text  
  - Converted to `Whole Number`  
- **Bathrooms**:  
  - Removed `" bathrooms"` text  
  - Converted to `Whole Number`  
- **Size**: Dropped (too many nulls)
- **location**: Extracted the exact locations in places where The location was many strings of characters eg "Westlands Area, Westalnds" to have only one named location eg "Westlands" by creating a new custom column.
- **Average Price**: Created later in Power BI using **DAX**

####  Owners Table
- **email**: No changes  
- **fullname**: No changes  
- **ownerid**: No changes  
- **ownershipdate**: Converted to `Date` type  
- **Phone Number**: No changes  
- **PropertyID**: No changes  

####  Taxes Table
- **Annual Tax Amount**: No changes  
- **PropertyID**: No changes  
- **Tax Rate**: No changes  
- **Average Tax**: Created in Power BI using **DAX**

####  Utilities Table
- **monthly electricity cost**: No changes  
- **monthly internet cost**: No changes  
- **monthly water cost**: No changes  
- **PropertyID**: No changes  
- **Total Utility Cost**: Created using **DAX**  
- **Average Utility Cost**: Created using **DAX**

> Finalize by clicking **Close & Apply** to load the transformed data.

---

### 4.  Data Model

The following **Many-to-One** relationships were created (with **Single** filter direction):

| From Table | Column      | To Table | Column      | Relationship   |
|------------|-------------|----------|-------------|----------------|
| Owners     | PropertyID  | Houses   | PropertyID  | Many-to-One →  |
| Taxes      | PropertyID  | Houses   | PropertyID  | Many-to-One →  |
| Utilities  | PropertyID  | Houses   | PropertyID  | Many-to-One →  |

---

### 5.  Dashboard Visualizations

| Visualization Type      | Description                                                           |
|-------------------------|-----------------------------------------------------------------------|
| **Card**                | Total Properties                                                      |
| **Card**                | Average Utility Cost                                                  |
| **KPI**                 | Tax Rate (Trend: Last Assessment Date, Tagrget= Average Tax Rate)     |
| **Doughnut Chart**      | Property by Location (Filtered by top N)                               |
| **Bar Chart**           | Average Tax by Location. (Filterd by top N)                           |
| **Table Visual**        | Property Details: PropertyID, Phone Number, Location, Price, Tax Rate |
| **Slicer**              | Filter by Location                                                    |

---

### 6.  DAX Measures

The following **measures** were created in Power BI using **DAX**:

```DAX
Average Price = AVERAGE(Houses[Price])

Average Tax = AVERAGE(Taxes[Tax Rate])

Total Utility Cost = 
SUM(Utilities[MonthlyElectricityCost]) +
SUM(Utilities[MonthlyWaterCost]) +
SUM(Utilities[MonthlyInternetCost])

Average Utility Cost = 
DIVIDE(
    [Total Utility Cost],
    DISTINCTCOUNT(Utilities[PropertyID])
)
```

---

### 7.  Dashboard Purpose

This dashboard provides a **comprehensive overview** of the **Kenya real estate housing market**, enabling users to:

- Monitor average prices, utility costs, and tax rates  
- Analyze data by location  
- Visualize property-level metrics with relational insight
- Track houses from owners from the Propeerty Table using the Owner's phone numbers  

---

### 8.  Insights
**PROJECT RELATED INSIGHTS**
- **Communicating in Visuals Using limited Space**: I learnt to communicate my findings in visuals using a limited page on the powe bi report view. This means I learnt to add only the necessary visuals required to make the report comprehensive. It also meant that adding unnecessary elements like axis titles would mean limiting the available visualizing space. In this case I only used a donut pie and a bar chart to represent the trends in my data as required and they communicated whatever was intended effectively without cluttering my dashboard.
- **Color Uniformity**:I learnt to make sure that i used uniform colours throughout my visuals focusing on data communications rather than colouring my dashbard with too many colours(UI design) which made the dashboard simple, neat and effective in communication.
- **KPIs**: I learnt that for KPIs to work effectively, thhey need a tend axis which is normally a date foemat and a target which is more or less a coparison value against the real value used on the KPI.
- **Modeling and Relationships**: I came to understand that when modelling or creating relationships between tables, it is important to keep the filter direction to **Single** to prevent unnecessary overlaps between your data. Also, I learnt the working of *many to one* relationships much more intuitively which would have been difficult to wrap my head around minus the project.
- **Data cleaning**: I have come to appreciate the power of data cleaning in visualization when I realized that minus extracting the exact locations, the data reprt obtained would contain erroneous findings.

  **DATA RELATED INSIGHTS**
- **High Utility Costs**: Some regions report significantly higher utility costs, suggesting potential inefficiencies or higher living standards. eg Lavington, Westlands and Kiambu Road
- **Tax Rate Clustering**: Tax rates tend to cluster around specific areas, indicating local government taxation strategies. eg Muthaiga, Vipingo and Kiambu Road.
- **Market Saturation**: Multiple properties listed within the same location point to competitive or oversupplied markets. eg there are 23 properties in Lavington and 8 in Westlands.

---

### 9.  Challenges

- **Filters**: Wrapping my head around setting up filters for donut pie and pie chart was not easy an I had to tweak it through Trial and error.
- **KPIs**: When I was starting to Visualize the data, i didnt know the working of a KPI and I had to read a whole article from Microsoft Power BI page.
- **Iterating through steps**: When visualizing for example, I realized i had some locations not well extracted and I had to transform my data back to power query ediitor so that I could come back to visualization. Ther was a data mismatch between the locations and therefore I had to go back to power query to remove the **County** column because it was not reliable for data visualization as it contained regions from another country yet my dashboard is on Real Estate Housing in Kenya.

---


