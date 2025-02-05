# SHANTA KOFFEE ANALYSIS (Infographic)

## Problem Statement

I recently completed a data analysis project, creating an infographic for my imaginary company, "Shanta Koffee," using Power BI.  In this analysis, I sought to understand several aspects of the business, including top-performing coffee products, top-performing product types, and peak revenue times. I also performed a forecast analysis for the next six months.  I'd like to thank Sean Chandler for providing the dataset and tutorial.


### Steps followed 

#### Modeling

- Step 1 : The dataset is loaded in the Power BI and the root file is .csv
- Step 2 : After the dataset is loaded, the original file is hidden from the front end using the disable load option.
- Step 3 : The references are created for the above file as fact and dimension tables.
- Step 4 : Following cleaning transformations are carried out:
- Step 5 : Cleaning operations are performed wherever necessary.

            
#### Evaluation
- Step 1 : Following Dax measures are created:

            average order size = AVERAGE(Kraken_Koffee_fact[Total Sales])

            most popular seller = CALCULATE(MIN(product_dim[product_detail]), FILTER(product_dim, [product transactions rank]=1))

            product sales rank = RANKX(ALLSELECTED(product_dim),[total sales],,DESC,Dense)

            product transactions rank = RANKX(ALLSELECTED(product_dim),[total transations],,DESC,Dense)

            top earner = CALCULATE(MIN(product_dim[product_detail]), FILTER(product_dim, [product sales rank]=1))

	          total products = count(product_dim[product_id])

            total sales = SUM(Kraken_Koffee_fact[Total Sales])

            total stores = DISTINCTCOUNT(store_dim[store_id])

            total transations = COUNT(Kraken_Koffee_fact[transaction_id])

            % of sales = DIVIDE([total sales], CALCULATE([total sales],ALLSELECTED(product_dim)))

            max aos marker = SWITCH(TRUE(),
                    [average order size]=[Max avg order size (Hour)],[average order size],
                    BLANK())

	    Max avg order size (Hour) = MAXX(ALLSELECTED(Kraken_Koffee_fact[Hour]),[average order size])

            Product Category Rank = CALCULATE(RANKX(ALL(product_dim[product_type]),[total sales],,DESC,Dense),ALL(product_dim[product_category]))

            bar chart label (product category) = FORMAT([total sales]/1000,"$###,##0k") & " (" & FORMAT([% of sales],"###,##0.0%") & ")"

            max aos marker label = "Max Order Size: " & FORMAT([max aos marker],"$###,##0.00")

            Top 10 Product Highlight = IF(
                                [Product Category Rank]<=10,"#F07F09",
                                "#1B587C")

            average daily sales = AVERAGEX(VALUES('forecast date table'[Date]),[total sales])

            average saily sales switch = SWITCH(TRUE(),
            [total sales]<>BLANK(),[total sales],
            [average daily sales 2])

            forecast = CALCULATE(SUMX(VALUES('forecast date table'[Date]),[average saily sales switch]),FILTER(ALLSELECTED('forecast date 				table'[Date]),'forecast date table'[Date]<=max('forecast date table'[Date])))

            forecast marker = SWITCH(TRUE(),
                    MAX('forecast date table'[month num])=12,[forecast],BLANK())

            
- Step 2: Following is the screenshot of the infographic and on the left, smart narrative is used to highlight the different parameters.

![Image](https://github.com/user-attachments/assets/3ed6011f-90a2-4cba-a1dd-b733daee38a0)
