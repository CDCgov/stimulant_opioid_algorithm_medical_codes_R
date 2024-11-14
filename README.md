# Guide to the Medical Code Component <p> of the NCHS Stimulant and Opioid Use Algorithm

**Author: Minchan (Daisy) Shi**

**sse6@cdc.gov**

**Edited: November 14, 2024**

## Overview

<ins> Background </ins>

The code contained in this repository is part of a project titled “Utilizing Natural Language Processing and Machine Learning to Enhance the Identification of Stimulant and Opioid-Involved Health Outcomes in the National Hospital Care Survey,” which was funded by the Office of the Secretary – Patient-Centered Outcomes Research Trust Fund in the 2023 fiscal year. This work was carried out by the National Center for Health Statistics (NCHS) using data from the 2020 National Hospital Care Survey. The full algorithm has two primary components: a natural language processing component and a medical code-based component. This R code covers the medical code-based component.

This algorithm uses medical codes (for example, diagnoses or procedure codes) to identify hospital encounters involving the non-therapeutic use of stimulants and opioids. Non-therapeutic use includes the use of illicit substances (stimulants or opioids), misuse of prescriptions, or the unspecified non-therapeutic use of substances. The algorithm is designed to analyze data in table form and produce an output file containing variables related to non-therapeutic stimulant use, non-therapeutic opioid use, and the co-occurrence (a proxy for co-use) of these two types of use. 

The natural language processing component for this algorithm designed to analyze clinical notes is in the following repository:

  * https://github.com/CDCgov/stimulant_opioid_algorithm_clinical_notes

<ins>Related repositories:</ins>

The stimulant algorithm is the third iteration of two previously developed substance-use-related algorithms. For your reference, these algorithms can be found in the following repositories.

  * [ ] Algorithm to detect opioid use, selected mental health issues, and substance use disorders in medical codes: 
    * https://github.com/CDCgov/Opioid_SUD_MHI_MedCodes

  * [ ] Algorithms to detect opioid use, selected mental health issues, and substance use disorders in clinical notes: 
    * https://github.com/CDCgov/Opioid_Involvement_NLP
    * https://github.com/CDCgov/SUD_MHI_NLP

<ins>Usage </ins>

The 'create_output_table function' is a key component of this algorithm. It generates an output table by applying mapping rules defined in an external Excel file. The function is flexible and capable of handling input data from various sources, including CSV, Excel, SAS, databases, or preloaded data. The mapping rules categorize and filter the input data based on specific columns and code systems, making it easier to identify stimulant and opioid involvement.

This document explains the parameters of the 'create_output_table' function in detail, providing usage scenarios and notes to guide users in integrating the function into their R projects. Depending on the specific needs of the analysis, examples can be tailored to fit different use cases or expanded to cover additional functionalities of the function.

Mappings are provided for categories such as use of stimulant opioid use, and a derived variable for co-use. The co-use variable is created when both opioid and stimulant use is detected in the same hospital encounter. The input data is processed through structured medical codes, searching for patterns of non-therapeutic stimulant and opioid use, and other related health outcomes.

The `create_output_table` function generates an output table based on mappings defined in a specified Excel file. It is designed to handle various types of input data (CSV, Excel, SAS, database, or preloaded data), applying mapping rules to categorize and filter data based on specified columns and code systems. The included Notebook provides detailed explanations of each parameter, examples of usage scenarios, and notes to help users effectively utilize the `create_output_table function` in their R projects. Adjust the examples and details as necessary to match specific use cases or additional functionalities of your function.


## Installation

Before using the function, ensure you have installed the required R packages:
  * install.packages("readxl")
  * install.packages("dplyr")
  * install.packages("haven")

## Function Parameters

### code_mapping_file
- **Description:** Path to the Excel file `Code_Mapping_GitHub.xlsx` containing code mappings. Along with the function, we will provide the code mapping file `Code_Mapping_GitHub.xlsx` in order to run the `create_output_table` function. It must specify the location of the file. if you already set the Working Directory in the previous section, then you only need to provide the input file name, and not the full path.
  
- **Example:** `"path/to/Code_Mapping_GitHub.xlsx"`  

### data_type
- **Description:** Type of input data. Supported types include "txt", "csv", "xls", "xlsx", "sas  ", "database", or "preloaded_data".
  
- **Examples:** `"txt"`, `"csv"`, `"xls"`, `"xlsx"`, `"sas"`, `"database"`, or `"preloaded_data"` 

### input_file
- **Description:** Path to the input file (depends on data_type). For "database" or "preloaded_data", provide "NULL"(not specifying any code system).
  
- **Examples:** `"path/to/input_file.csv"`, `NULL`

### conn_str
- **Description:** Connection string for accessing a database (required if data_type is "database"),provide "NULL" if data_type is not "database".
  
- **Example:** `"Driver={ODBC Driver 17 for SQL Server};Server=your_server;Database=your_database;Trusted_Connection=yes;"`

### query
- **Description:** SQL query to retrieve data from a database (required if data_type is "database"),provide "NULL" if data_type is not "database".
  
- **Example:** `"SELECT * FROM your_table"`

### code_system_name
- **Description:** Name of the code system to filter mappings. Users can choose from "ICD-10-CM", " ", "HCPCS", "RXNORM", "SNOMED", "LOINC", or NULL (not specifying any code system).
  
- **Examples:** ` c("ICD-10-CM", "HCPCS")`  or NULL

Note: The codes for the included code systems are intended to search for the following types of information:


| Code System |	Information Type |
| ------ | ------ | 
| ICD-10-CM |	Diagnoses |
| SNOMED	| Diagnoses |
| HCPCS |	Procedures |
| RXNORM |	Medications |
| LOINC |	Labs |

### columns_to_keep
- **Description:** Columns to include in the output table.
  
- **Examples:** `c("ID", "VisitType")`

### code_variable
- **Description:** Name of the column containing codes to match against mappings.(only allow one variable)
  
- **Examples:** `"ICD_Code"`, `"RXNORM_Code"`

### subset_string (optional)
- **Description:** Condition to subset rows of the input data.
  
- **Examples:** `CodeType == "ICD-10-CM"`
  
- **The subset_string parameter** is used to filter rows of the input data based on the specified condition.
  
- **subset_string = 'VisitType != "NA"' ** filters the data to include only rows where the VisitType column does not equal "NA".
  
- **The single quotes** around the condition are necessary to ensure it is correctly parsed as a string.

### results_file  
- **Description:**:  Path to the output file. The output is only available in CSV format, so the file must have a `.csv` extension. 
- **Examples:** `"results_file.csv"`


## Notes

- An example of the R function `create_output_table` is provided in the code file itself. Other than the adjusting the function parameters for your use case, the code is fixed and should not be modified. You can run it as-is in this section.

- Ensure that your input data and code mapping file are correctly formatted as per the function's requirements.
  
- Customize columns_to_keep and subset_string parameters based on your specific data and analysis needs.

- For "database" data type, provide a valid connection string (conn_str) and SQL query (query) to fetch data from your database.  




## Public Domain Standard Notice
This repository constitutes a work of the United States Government and is not
subject to domestic copyright protection under 17 USC § 105. This repository is in
the public domain within the United States, and copyright and related rights in
the work worldwide are waived through the [CC0 1.0 Universal public domain dedication](https://creativecommons.org/publicdomain/zero/1.0/).
All contributions to this repository will be released under the CC0 dedication. By
submitting a pull request you are agreeing to comply with this waiver of
copyright interest.

## License Standard Notice
The repository utilizes code licensed under the terms of the Apache Software
License and therefore is licensed under ASL v2 or later.

This source code in this repository is free: you can redistribute it and/or modify it under
the terms of the Apache Software License version 2, or (at your option) any
later version.

This source code in this repository is distributed in the hope that it will be useful, but WITHOUT ANY
WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A
PARTICULAR PURPOSE. See the Apache Software License for more details.

You should have received a copy of the Apache Software License along with this
program. If not, see http://www.apache.org/licenses/LICENSE-2.0.html

The source code forked from other open source projects will inherit its license.

## Privacy Standard Notice
This repository contains only non-sensitive, publicly available data and
information. All material and community participation is covered by the
[Disclaimer](DISCLAIMER.md)
and [Code of Conduct](code-of-conduct.md).
For more information about CDC's privacy policy, please visit [http://www.cdc.gov/other/privacy.html](https://www.cdc.gov/other/privacy.html).

## Contributing Standard Notice
Anyone is encouraged to contribute to the repository by [forking](https://help.github.com/articles/fork-a-repo)
and submitting a pull request. (If you are new to GitHub, you might start with a
[basic tutorial](https://help.github.com/articles/set-up-git).) By contributing
to this project, you grant a world-wide, royalty-free, perpetual, irrevocable,
non-exclusive, transferable license to all users under the terms of the
[Apache Software License v2](http://www.apache.org/licenses/LICENSE-2.0.html) or
later.

All comments, messages, pull requests, and other submissions received through
CDC including this GitHub page may be subject to applicable federal law, including but not limited to the Federal Records Act, and may be archived. Learn more at [http://www.cdc.gov/other/privacy.html](http://www.cdc.gov/other/privacy.html).

## Records Management Standard Notice
This repository is not a source of government records, but is a copy to increase
collaboration and collaborative potential. All government records will be
published through the [CDC web site](http://www.cdc.gov).

## Additional Standard Notices
Please refer to [CDC's Template Repository](https://github.com/CDCgov/template) for more information about [contributing to this repository](https://github.com/CDCgov/template/blob/main/CONTRIBUTING.md), [public domain notices and disclaimers](https://github.com/CDCgov/template/blob/main/DISCLAIMER.md), and [code of conduct](https://github.com/CDCgov/template/blob/main/code-of-conduct.md).



