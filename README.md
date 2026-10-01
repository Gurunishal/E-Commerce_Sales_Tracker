# Company Sales
ABAP on Cloud 

# A simply analytic application to track E-commerce company's sales:

- Total sales per currency
- Total sales per customer
- Total sales per product category
- Total quantity sold per country

  - Sales Order Header + Sales Order Item = I_SO **(#BASIC View) (Fact)
  - BPA = I_BPA **(#BASIC View) (Dimension)
  - Prod = I_PROD **(#BASIC View) (Dimension)
    - I_PROD + I_SO = I_CO_SALES **(#COMPOSITE View)
    - I_BPA + I_CO_SALES =  C_SLS_ANA **(Consumption View)
        - CO_SPC (Total Sales per currency)
        - CO_SPCust (Total sales per customer)
        - CO_QPC (Total quantity sold per country)

In the context of analytics:
- Master Data = Dimension
- Transaction Data = Fact
- Master + Transaction = CUBE

- Total Sales per Currency:

<img width="3024" height="1964" alt="image" src="https://github.com/user-attachments/assets/c8432af2-3270-4fba-a3be-ddbef940d0f5" />

- Total Sales per Customer:

<img width="3024" height="1964" alt="image" src="https://github.com/user-attachments/assets/abf823a7-da63-41c9-a533-c522b0c4ac39" />

- Total sales per Product Category:
    
<img width="3024" height="1964" alt="image" src="https://github.com/user-attachments/assets/6323db02-8e76-40e8-8bbf-3e9c8ea5d93e" />

- Total Quantity sold per Country:

<img width="3024" height="1964" alt="image" src="https://github.com/user-attachments/assets/dd1dffd0-b245-4890-8729-5b7992d575c5" />
