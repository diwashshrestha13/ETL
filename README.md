


# Fleet (Delivery Truck) -ETL/Dashboard


## Problem Statement

The dashboard was created to understand the company's financial position, vehicle and driver performance, and customer demographic. The dashboard can help stakeholder understand it's financial trend, seasonality, cost and revenue generation from both vehicles and drivers, and market. 

With the quantifiable measures such as revenue, operating cost ratio, growth rate, average revenue based on various factors, and revenue earned from different geographic variable, the company can identify it's strength and weakness and take actionable measures to improve it's both profit and revenue. 


### Steps followed 
- ETL process
The process includes extracting data from Kaggle API, transforming in Python, and loading the data in SQL, and connecting it to Power BI for further transformation on the visualization needs. 
![Image](https://github.com/user-attachments/assets/dc001e47-5005-4314-a439-f701f2d1b0e0)
- Data Collection
The data was collected using Kaggle API with python. The data contained three files, an excel workbook with 3 sheets as dimension table including customer, driver, and truck, an excel file containing cost fact table, and a csv containing freight information.  

- Data Cleaning in Python

The data was cleaned in python using the pandas library. 

Fact Table Separation:
The fact table was split into separate tables to improve data structure and facilitate analysis.

Null Value Handling:
All tables were examined for null values, which were removed to ensure data integrity.

Column Standardization:
Columns across all tables were standardized to ensure consistency and alignment with the schema.

Cost Sheet Cleanup:
The cost sheet contained two tables in a single sheet, separated by year. These tables were cleaned, with null values removed, and formatted to adhere to a standardized structure suitable for SQL import.

Freight Table Standardization:
The freight table’s columns were standardized to match the conventions used in the other tables, ensuring consistency across the schema.

- Data injection into SQL
Data Import:
Data was imported into the SQL database using SQLAlchemy. The import process involved:

Extracting cleaned and standardized data.
Using SQLAlchemy's to_sql() method to load the data into the corresponding tables in the PostGresSQL database.
Table Structure and Relationships: After importing the data, the database schema was further refined:
![Image](https://github.com/user-attachments/assets/702a3f53-94c8-4176-9527-e44623271cba)
Primary Keys:
A primary key was created for each table to uniquely identify each record. This was achieved using SQLAlchemy's PrimaryKeyConstraint.
Foreign Keys:
Foreign keys were established between related tables to maintain data integrity and establish relationships. This was done using SQLAlchemy’s ForeignKey constraint to link the relevant columns across tables.

Wrote SQL queries to derive insights, understand the data and answer business questions. Applied aggregations, joins, CTE, and Windows Function to extract key metrics.

- Data Load to Power BI

Established a connection between PostGresSQL and Power BI by importing and structuring the data for visualization.
- Data Modeling

- Created relationships between tables.
- Built a Calendar Table for time-based analysis (date filtering, YTD, MTD calculations).
- A Region Table was created to facilitate geographic-based analysis by grouping data at a higher level of granularity. The Region Table was designed to summarize findings on a macro level, allowing for easier analysis and reporting across larger geographical groupings.

- Dashboard & Metrics

Designed a Power BI dashboard with key insights and visuals separted into 3 dashboard.Created DAX measures for KPIs like average KPI's, growth rate, ratios, filters, map visualization, ranking, and matrix tables. 
           
- Financial Overview Dashboard. 
![Image](https://github.com/user-attachments/assets/82febd73-f5cb-4b0d-8c78-576ed3b8a70a)
The dasboard KPI section shows total revenue, profit, operating cost ratio and year growth rate (an arrow indicating green when positive and red when negative)

A line graph was added to compare the trend of both cost and profit. 

A table was created to show present month growth rate compared to last (also arrow indicating green when positive and red when negative)

- Vehicle and Driver Performance
![Image](https://github.com/user-attachments/assets/d967b6ca-668f-4a82-97b7-e6a21afddf4c)
The KPI section contains total distance traveled, number of orders, revenue per KG, Avg fuel consumer per KM, and Avg revenue earned per delivery. 

A table was created to show the distributed percentage of revenue, delivery, cost, and profit percentage among various categories of vehicle. 


- Customer Insight

![Image](https://github.com/user-attachments/assets/88d251ac-2783-499e-9ec6-21ada5b8e1da)

A state map was to show the reveue earned from each state.

A table was created to rank the state based on revenue earned ,and number of customers and Avg earned per Customer. 

A line graph was added to see the number of order trend based on date and a hollow pie chard was added to depict the number of orders from region. 
        
   ### DAX measures used:

- Following DAX expression was to calculate the growth in profit every month compared to last month. A conditional icon was also added to visually depict the growth or decline in profit. 

        Growth_Rate_Profit = 
        VAR TotalProfit = [Total_Profit]
        VAR PreviousMonthRevenue = [PreviousMonthProfit]  
        RETURN
        IF(
        NOT ISBLANK(PreviousMonthRevenue) && PreviousMonthRevenue <> 0, 
        ((TotalProfit - PreviousMonthRevenue) / [PreviousMonthRevenue]),
        BLANK()
        )

![Image](https://github.com/user-attachments/assets/feb5bb4b-2402-4266-858b-2d415f57593a)

- Certain DAX measures were created to understand the cost and revenue distribution and different type of vehicle categories. 

![Image](https://github.com/user-attachments/assets/d6be1428-d55e-40f9-ab75-842fc757c34a)

        

 
Following Dax was used to find  average time it took to complete a delivery.
 
         Truvck_Delivery_% = 
            VAR total_order = COUNTROWS('freight')

            RETURN 
        DIVIDE(total_order, [No_of_Order], 0) 

- A rank table was created to rank a state based on revenue.  
![Image](https://github.com/user-attachments/assets/97108563-7f7e-4ef1-bd57-9bded240210b)

        StateRevenueRank = 
        RANKX(
            ALL('RegionTable'[State]),
            CALCULATE(SUM('freight'[net_revenue])),
            , 
            DESC,  
            Dense   
        )
The report was then published to Power BI Service.

# Insights

Following inferences can be drawn from the dashboard;

### [1] Highest Profit Growth

the highest profit growth is usually in February, and between end of quarter 3 and start of quarter 4 suggestign a seasonality. The company would benifit more looking for the cause of surge in profit. 

           
### [2] Vehicle Performance
   
The company earns high revenue from Trailer type vehicles. Likewise, Box Fridge vehicles accounts for lowest revenue and it also has the highest mileage among all the vehicle types. Lastly, although tractor accounts for 9.29% profit, it earns $714 revenue per km. The company might consider replacing Box Friedge with more Tractor if there is a demand to be fulfilled. 

  
  ### [3] Revenue per Category  
West Virginia followed by Oklahoma and South Carolina accounts for the top 3 revenue by state in that order. Likewise, the company has the highest revenue from West region and least from Northeast region. Although, WA ranks 8 in revenue earned from state, it has the highest avg revenue per customer with a total of 91 customer.           

### Some Business Question Solved through SQL Query
Which driver has the highest fuel efficiency?

![Image](https://github.com/user-attachments/assets/3c16906a-0b4e-4655-8cad-fb5232d02dc3)


What truck has the highest maintenance cost per km?

![Image](https://github.com/user-attachments/assets/968baf94-709b-4f2c-b7e5-1c83fc502ebc)


What is the correlation between vehicle age and maintenance cost?
![Image](https://github.com/user-attachments/assets/32e9c5af-c4b1-47af-a151-2e85917f387a)


Is there seasonality in order value?
![Image](https://github.com/user-attachments/assets/e8156305-02af-4dc1-934a-0c725155454e)
![Image](https://github.com/user-attachments/assets/39cc2f02-cbdf-4076-9a78-99d43e48759f)

What is the maintenance cost based on total km?
![Image](https://github.com/user-attachments/assets/b649f675-ce11-45cc-ae7a-d62d20322e01)
