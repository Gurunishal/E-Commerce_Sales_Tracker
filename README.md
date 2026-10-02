# E-Commerce Sales Tracker

An ABAP analytics prototype for exploring e-commerce sales by currency, customer, product category, and country. It is built with ABAP CDS analytical views on SAP BTP ABAP Environment (ABAP Cloud) and includes a small generated dataset for demonstration.

## Business problem

An e-commerce business needs a simple way to understand sales performance across products and customer markets. This project models order items as transaction facts, enriches them with product and customer dimensions, and exposes aggregated data for analysis.

## Features

- Analyze gross sales by currency, customer, and product category.
- Review sold quantity by customer country.
- Combine order item facts with product and business partner dimensions.
- Use an analytical cube with aggregation annotations and a consumption query.
- Generate demonstration master and transaction data from an ABAP class.

## Analytics screenshots

### Total sales by currency

![Total sales by currency](https://github.com/user-attachments/assets/c8432af2-3270-4fba-a3be-ddbef940d0f5)

### Total sales by customer

![Total sales by customer](https://github.com/user-attachments/assets/abf823a7-da63-41c9-a533-c522b0c4ac39)

### Total sales by product category

![Total sales by product category](https://github.com/user-attachments/assets/6323db02-8e76-40e8-8bbf-3e9c8ea5d93e)

### Quantity sold by country

![Quantity sold by country](https://github.com/user-attachments/assets/dd1dffd0-b245-4890-8729-5b7992d575c5)

## Architecture

```text
ZATS_GS_SO_HDR (order header) ─┐
                               ├─ ZI_ATS_GS_SALES (basic order-item fact)
ZATS_GS_SO_ITEM (order items) ─┘
                                          │
ZATS_GS_PROD ─ ZI_ATS_GS_PROD (product dimension)
                                          │
                              ZI_ATS_GS_CO_SALES (composite fact)
                                          │
ZATS_GS_BPA ─ ZI_ATS_GS_BP (customer dimension)
                                          │
                              ZI_ATS_GS_CO_SLS_CUBE (analytical cube)
                                          │
                              ZC_ATS_GS_TOT_SALES (consumption query)
```

| Layer | CDS entity or table | Purpose |
|---|---|---|
| Persistence | `ZATS_GS_SO_HDR`, `ZATS_GS_SO_ITEM` | Sales order headers and line items |
| Persistence | `ZATS_GS_PROD` | Product master data, category, price, and discount |
| Persistence | `ZATS_GS_BPA` | Business partner data; the CDS dimension filters to customers |
| Basic | `ZI_ATS_GS_SALES` | Order item fact data |
| Basic | `ZI_ATS_GS_PROD`, `ZI_ATS_GS_BP` | Product and customer dimensions |
| Composite | `ZI_ATS_GS_CO_SALES` | Sales fact enriched with product and buyer attributes |
| Cube | `ZI_ATS_GS_CO_SLS_CUBE` | Analytical cube with sales and quantity aggregation |
| Consumption | `ZC_ATS_GS_TOT_SALES` | Analytical query for reporting |

## Sample data

`ZCL_ATS_GS_DATAGENERATOR` is an ABAP console application class that creates sample customers, products, 50 sales orders, and 100 order items. It generates random customer/order associations and uses UUIDs for identifiers.

> **Important:** Running the generator starts by deleting all rows from its four custom tables (`ZATS_GS_BPA`, `ZATS_GS_PROD`, `ZATS_GS_SO_HDR`, and `ZATS_GS_SO_ITEM`) before inserting fresh demo data. Run it only in a development or training client where those records are disposable.

## Requirements

- SAP BTP ABAP Environment or another ABAP system with compatible ABAP Cloud and CDS analytical-query support.
- ABAP Development Tools (ADT) for Eclipse.
- Authorization to import and activate the repository objects, create the custom tables, and run the data generator.

This is an ABAP backend project. It is not a standalone application with a local web-server start command. The repository does not include a Fiori/UI project; use an analytical preview or reporting consumer available in your SAP environment to explore the query.

## Import and run

1. Connect to the target ABAP system from ADT.
2. Import the repository with abapGit or the import process provided by your environment.
3. Activate the domains and data elements, structure and tables, CDS entities, and ABAP classes. Resolve package or namespace differences required by the system.
4. Run `ZCL_ATS_GS_DATAGENERATOR` as an ABAP Console Application from ADT to populate the demo tables. Remember that it clears the four tables listed above first.
5. Open `ZC_ATS_GS_TOT_SALES` in the analytical preview supported by your system, or consume the query from a compatible analytics client.

Activation and analytical preview options depend on the ABAP release and training-system configuration.

## Repository structure

```text
src/
  zats_gs_*                       Custom tables, domains, data elements, structure
  zi_ats_gs_sales.*               Basic sales fact CDS view
  zi_ats_gs_prod.*                Basic product dimension CDS view
  zi_ats_gs_bp.*                  Basic customer dimension CDS view
  zi_ats_gs_co_sales.*            Enriched composite fact CDS view
  zi_ats_gs_co_sls_cube.*         Analytical cube CDS view
  zc_ats_gs_tot_sales.*           Analytical consumption query
  zcl_ats_gs_datagenerator.*      Demo data generator
```
