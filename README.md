# Scalable_HW1
# Data Validation and Transformation via Polars DataFrames

## Overview

This project is designed to support the procurement process for server racks in the supply chain of a hyperscale cloud provider. The primary goal is to validate, transform, and analyze quote data provided by multiple vendors to ensure accuracy and consistency in server rack component costs. Using Polars DataFrames, we process, merge, and validate data from various sources to produce actionable summary tables for cost assessment and reporting.


## Table of Contents
1. [Overview](#overview)
2. [Project Structure](#project-structure)
3. [Data Validation Requirements](#data-validation-requirements)
4. [Methodology](#methodology)
   - [Step 1: Data Preprocessing](#step-1-data-preprocessing)
   - [Step 2: Data Transformation](#step-2-data-transformation)
      - [Calculating Server Rack Quantities](#calculating-server-rack-quantities)
      - [Calculating Quote Summaries](#calculating-quote-summaries)
   - [Step 3: Validation](#step-3-validation)
5. [Final Outputs](#final-outputs)
6. [Instructions for Use](#instructions-for-use)

## Project Structure

- **Input Files**:
  - `quote_lines.csv`: Detailed quote line items provided by vendors for each server and rack component.
  - `quote_summaries.csv`: Summary quotes provided by vendors, representing the total cost for each program.
  - `server_components.xlsx`: Quantity data for server components.
  - `rack_components.xlsx`: Quantity data for rack components.

- **Output Tables**:
  1. **Total Server Rack Quantity by Component**: Total extended quantity of each component across programs.
  2. **Total Cost Per Program Per Vendor (Latest Quote)**: Cost summary based on the most recent quote per vendor and program.
  3. **Total Cost Per Program Per Vendor (First Quote)**: Cost summary based on the first received quote per vendor and program.
  4. **Best-in-Class Pricing Per Program**: The lowest total component price across vendors for each program.

## Data Validation Requirements

The data validation process ensures that:
1. **Date Range**: Quotes are accepted only if submitted between the first Monday and the 25th of each month.
2. **Consistency**: The sum of detailed quote line items must match the summary total provided by vendors. Quotes with discrepancies are excluded.
3. **Program-Vendor Exclusions**: Vendor 7's quotes for Programs C and E are automatically discarded.

## Methodology

### Step 1: Data Preprocessing

- Load and standardize column names from `quote_lines.csv` and `quote_summaries.csv`.
- Convert `quote_timestamp` columns to datetime format to enable date-based filtering and grouping.
- Relabel program codes (`A`, `B`, `C`, etc.) to full names (`Program_A`, `Program_B`, etc.) for consistency.

### Step 2: Data Transformation

#### Calculating Server Rack Quantities
1. **Unpivot**:
   - Transform component columns in `filtered_quote_lines_df1` to a vertical format (longer format) to facilitate merging and scaling.
2. **Merge**:
   - Merge unpivoted tables to calculate total costs for each component by multiplying quantity by unit price, producing the **Table of Scaled Cost Per Component**.
3. **Pivot and Sum**:
   - Re-pivot the data back to a wide format and aggregate component totals by program.

#### Calculating Quote Summaries
1. **Latest and First Quote Costs**:
   - Group by `program` and `vendor` and retrieve the latest and first `quote_timestamp` for each group.
   - Calculate total costs based on both the latest and first quotes received per vendor-program combination.
2. **Best-in-Class Pricing**:
   - Determine the lowest component price across all vendors for each program, yielding a total "best-in-class" price.

### Step 3: Validation

To ensure data accuracy, we:
- Merge `scaled_quote_lines` with `filtered_quote_summaries_df`.
- Compare the `reported_total_price` with the calculated `total_cost` for each entry, allowing a small tolerance (`epsilon`) to account for rounding differences.
- Filter out records that do not meet this validation criteria to generate a validated dataset.

## Final Outputs

The final outputs, included as the last cells in the Jupyter notebook, are:
1. **Extended Quantities Table**: Detailed component quantities across programs.
2. **Total Cost (Latest Quote)**: Total cost per program per vendor based on the latest received quote.
3. **Total Cost (First Quote)**: Total cost per program per vendor based on the first received quote.
4. **Best-in-Class Total Cost**: Lowest possible cost for each program, regardless of vendor.

## Instructions for Use
Ensure Polars and Pandas libraries are installed in your Python environment. You can install them using:
```bash
pip install polars pandas
```
Ensure you have Polars, Pandas, and other necessary libraries installed.


