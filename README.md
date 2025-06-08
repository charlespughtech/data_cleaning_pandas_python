# Customer Call List Data Cleaning with Pandas

### *data_cleaning_pandas_python*

---

**Author:** Charles Pugh  
Google-certified Data Analyst  
Email: [charlespughtech@gmail.com](mailto:charlespughtech@gmail.com)  
LinkedIn: [https://www.linkedin.com/in/charlespughtech/](https://www.linkedin.com/in/charlespughtech/)  
Date: June 08, 2025

---

## Table of Contents

- [Project Overview](#project-overview)
- [Requirements](#requirements)
- [Project Structure](#project-structure)
- [Data Cleaning](#data-cleaning)
  - [1. Remove Duplicates](#1-remove-duplicates)
  - [2. Drop Unnecessary Column](#2-drop-unnecessary-column)
  - [3. Clean Last Names](#3-clean-last-names)
  - [4. Standardize Phone Numbers](#4-standardize-phone-numbers)
  - [5. Split Address Column](#5-split-address-column)
  - [6. Standardize Paying Customer and Do Not Contact](#6-standardize-paying-customer-and-do-not-contact)
  - [7. Handle Missing Values](#7-handle-missing-values)
  - [8. Filter Rows](#8-filter-rows)
  - [9. Reset Index](#9-reset-index)
- [Usage](#usage)
- [Important Note on File Paths](#important-note-on-file-paths)
- [Contact](#contact)

---

## Project Overview

This project involves cleaning a Customer Call List dataset using Pandas to prepare it for a contact center. The dataset, sourced from a CSV file, requires several cleaning steps to address issues such as duplicates, inconsistent formatting, missing values, and irrelevant data. The final cleaned dataset is saved as an Excel file for further use.

---

## Requirements

- Python 3.8 or higher
- Jupyter Notebook
- Pandas library

---

## Project Structure

```bash
data_cleaning_pandas_python/
├── README.md
├── data_cleaning_pandas.ipynb
└── data_cleaning_pandas.html
```

---

## Data Cleaning

The data cleaning process involves the following steps:

### 1. Remove Duplicates

Duplicates in the dataset are identified and removed to ensure each record is unique.

```python
df = df.drop_duplicates()
```

### 2. Drop Unnecessary Column

The column `'Not_Useful_Column'` is removed as it does not provide relevant information for the contact center.

```python
df = df.drop(columns='Not_Useful_Column')
```

### 3. Clean Last Names

Non-alphanumeric characters are stripped from the `'Last_Name'` column to standardize the data.

```python
df['Last_Name'] = df['Last_Name'].str.strip('./_')
```

### 4. Standardize Phone Numbers

Phone numbers are cleaned by removing non-numeric characters and formatted to a standard format (e.g., 123-456-7890).

```python
df['Phone_Number'] = df['Phone_Number'].str.replace('[^a-zA-Z0-9]', '', regex=True)
df['Phone_Number'] = df['Phone_Number'].apply(lambda x: str(x))
df['Phone_Number'] = df['Phone_Number'].apply(lambda x: x[0:3] + '-' + x[3:6] + '-' + x[6:10])
df['Phone_Number'] = df['Phone_Number'].str.replace('nan--', '').str.replace('Na--', '')
```

### 5. Split Address Column

The `'Address'` column is split into three separate columns: `'Street_Address'`, `'State'`, and `'Zip_Code'`.

```python
df[['Street_Address', 'State', 'Zip_Code']] = df['Address'].str.split(',', n=2, expand=True)
df = df.drop(columns='Address')
```

### 6. Standardize Paying Customer and Do Not Contact

Values in the `'Paying_Customer'` and `'Do_Not_Contact'` columns are standardized to `'Y'` for Yes and `'N'` for No.

```python
df['Paying_Customer'] = df['Paying_Customer'].str.replace('Yes', 'Y').str.replace('No', 'N')
df['Do_Not_Contact'] = df['Do_Not_Contact'].str.replace('Yes', 'Y').str.replace('No', 'N')
```

### 7. Handle Missing Values

Missing values (`NaN`, `N/a`) are replaced with empty strings to clean the dataset.

```python
df = df.replace({'NaN': '', 'N/a': ''}).fillna('')
```

### 8. Filter Rows

Rows where `'Do_Not_Contact'` is `'Y'` or `'Phone_Number'` is empty are removed, as these records are not useful for the contact center.

```python
for x in df.index:
    if df.loc[x, 'Do_Not_Contact'] == 'Y':
        df.drop(x, inplace=True)
for x in df.index:
    if df.loc[x, 'Phone_Number'] == '':
        df.drop(x, inplace=True)
```

### 9. Reset Index

The index is reset to provide a clean, sequential index for the final dataset.

```python
df = df.reset_index(drop=True)
```

---

## Usage

To replicate this project:

1. Clone the repository:
   ```bash
   git clone https://github.com/charlespughtech/data_cleaning_pandas_python.git
   cd data_cleaning_pandas_python
   ```

2. Install the required libraries:
   ```bash
   pip install pandas
   ```

3. Open the Jupyter Notebook:
   ```bash
   jupyter notebook data_cleaning_pandas.ipynb
   ```

4. **Update the file paths** in the notebook to match the location of the files on your computer (see [Important Note on File Paths](#important-note-on-file-paths)).

5. Run the cells sequentially to perform the data cleaning steps.

6. The cleaned data will be saved in the specified location on your computer.

---

## Important Note on File Paths

The file paths used in the code are specific to the author's computer. You will need to modify these file locations to match the file locations on your own PC for the following parts of the code:

- **Reading the input file with `pd.read_csv()`**: Replace the file path in the code with the actual location of the CSV file on your PC.
  - For example, if your input CSV file is located at `D:\data\project\Customer Call List.csv`, update the line to:
    ```python
    df = pd.read_csv(r"D:\data\project\Customer Call List.csv")
    ```

- **Saving the output file with `df.to_excel()`**: Replace the file path with the desired location and filename for the cleaned Excel file on your PC.
  - For example, to save the output to `D:\data\project\cleaned\customer_call_list_clean.xlsx`, use:
    ```python
    df.to_excel(r"D:\data\project\cleaned\customer_call_list_clean.xlsx", index=False)
    ```

Ensure that you use the correct file extensions and names as per your files.

---

## Contact

For any inquiries or further information, please contact:

**Charles Pugh**  
Google-certified Data Analyst  
Email: [charlespughtech@gmail.com](mailto:charlespughtech@gmail.com)  
LinkedIn: [https://www.linkedin.com/in/charlespughtech/](https://www.linkedin.com/in/charlespughtech/)

---

*This README.md was generated on June 08, 2025, at 10:41 PM BST.*
