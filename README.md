# Scalable_HW1
# Data Validation and Transformation via Polars DataFrames

## Overview

This project is designed to support the procurement process for server racks in the supply chain of a hyperscale cloud provider. The primary goal is to validate, transform, and analyze quote data provided by multiple vendors to ensure accuracy and consistency in server rack component costs. Using Polars DataFrames, we process, merge, and validate data from various sources to produce actionable summary tables for cost assessment and reporting.


## Table of Contents
1. [Overview](#overview)
2. [Project Structure](#project-structure)
3. [Data Validation Requirements](#data-validation-requirements)
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

## Final Outputs

The final outputs, included as the last cells in the Jupyter notebook and in folder `output_tables`, are:
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


