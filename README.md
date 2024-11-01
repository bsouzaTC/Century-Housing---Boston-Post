# Rent Roll File Processing

This repository contains two Python scripts for processing Rent Roll files, used in a complementary way. The first script processes and formats the initial data to preserve all the information from the original report, while the second script performs additional cleaning and inserts the final occupancy percentage calculation, making the file ready for ingestion.

The division into two scripts allows validation of the transformed content, ensuring that the final information matches the data received in the original report. Below, you will find a detailed description of each script, a summary of the collected data, and step-by-step instructions for sequential execution.

## Script Structure

### 1. First Script: Initial Processing

The first script reads the original `.xls` files and applies various formatting to organize the data, ensuring complete preservation of the original information.

#### Features
- **Date Formatting**: Converts date values to the `MM/DD/YYYY` format, preserving empty cells.
- **Numeric Value Formatting**: Rounds numeric values to two decimal places.
- **Property Identification**: Automatically identifies the property name based on the file name.
- **Weekly File Generation**: Creates files for each day of the week (Monday to Sunday) with a fixed timestamp.
- **"Lease To" Column Cleanup**: Cleans unnecessary values, such as hours and zeros, in the "Lease To" column.
- **Total Calculations**: Adds a totals row at the end of the file with column sum totals.

### 2. Second Script: Final Cleanup and Occupancy Calculation

The second script processes the `.csv` files generated by the first script, removing unnecessary rows, adjusting column formats, and preparing the data for ingestion.

#### Features
- **Removing Rows with "Parking"**: Removes rows that contain the word "Parking" in the `BD/BA` column.
- **Cleaning the "Tenant" Column**: Replaces values like "--VACANT--" and "VACANT" with blank cells.
- **Unit Count**: Replaces the value "Totals" in the last row of the `Unit` column with the total number of units.
- **Formatting the `BD/BA` Column**: Adjusts values to follow the `number/1.0` format.
- **Character Removal in `Unit` Column**: Removes letters and symbols in the `Unit` column, keeping only numbers.
- **Occupancy Percentage Calculation**: Calculates the occupancy percentage based on "Current" values in the `Status` column and adds the final value in the last row.

## Summary of Collected Data and Transformations

| Final Column                | Source in Original Report        | Description                                                                                       | Transformations in Second Script |
|-----------------------------|-----------------------------------|---------------------------------------------------------------------------------------------------|----------------------------------|
| **Unit**                    | Column A                         | Unit number.                                                                                      | Removes letters and symbols from row 4 onwards, replaces "Totals" with total number of units |
| **BD/BA**                   | Column I                         | Unit type (e.g., 1/1.0 or 2/1.0).                                                                 | Adjusts the value to follow the `number/1.0` format |
| **Tenant**                  | Column B                         | Tenant name.                                                                                      | Replaces values like "--VACANT--" and "VACANT" with blank cells |
| **Status**                  | Column K                         | Unit status (e.g., "Current" or "Vacant-Unrented").                                               | Calculates occupancy percentage based on "Current" values and adds "X% Occupied" in the last row |
| **Sq. Ft.**                 | Column J                         | Unit area in square feet.                                                                         | No additional transformation |
| **Market Rent**             | Column L                         | Market rent for the unit.                                                                         | No additional transformation |
| **Rent**                    | Sum of columns N and O           | Current rent (sum of rent columns).                                                               | No additional transformation |
| **Deposits**                | Sum of columns Q and R           | Current deposits.                                                                                 | No additional transformation |
| **Lease From**              | Column F                         | Lease start date.                                                                                 | No additional transformation |
| **Lease To**                | Column H                         | Lease end date (formatted).                                                                       | No additional transformation |
| **Move-in**                 | Column F                         | Tenant move-in date (same as "Lease From" date).                                                  | No additional transformation |
| **Move-out**                | N/A                              | Tenant move-out date (filled later if applicable).                                                | No additional transformation |
| **Past Due**                | Column S                         | Past due amount.                                                                                  | No additional transformation |
| **NSF Count**               | N/A                              | Non-sufficient funds count (defaulted to '0').                                                    | No additional transformation |
| **Late Count**              | N/A                              | Late payment count (defaulted to '0').                                                            | No additional transformation |
| **Next Rent Increase Date** | N/A                              | Next rent increase date (left blank by default).                                                  | No additional transformation |
| **Next Rent Increase Amount** | N/A                            | Next rent increase amount (left blank by default).                                                | No additional transformation |

## Execution Instructions

1. **First Script**: 
   - Configure `input_directory` and `output_directory` with the correct paths.
   - Set the start date (`YYYY-MM-DD`) to generate weekly files.
   - Run the script to generate formatted `.csv` files in the output folder.

2. **Second Script**: 
   - Configure `input_folder` and `output_folder` with the directories where the input and output `.csv` files are located.
   - Run the script to clean and adjust the data for ingestion, generating the final files in the specified output folder.

The final files will be ready for use after the sequential execution of both scripts.
