# package-lock.json to CSV Converter

## Overview

This Python script converts a `package-lock.json` file into a CSV file containing detailed information about project dependencies, including their versions, integrity, and metadata. It also fetches the latest versions of the packages from the NPM registry.

---

## Features

- **Parse JSON:** Reads and processes `package-lock.json` into a structured DataFrame using Pandas.
- **Extract Key Information:** Captures current version, integrity hash, requirements, and nested dependencies.
- **Check Dependencies:** Identifies if a package has dependencies.
- **Fetch Latest Versions:** Uses the NPM registry API to get the most recent version of each package.
- **Export to CSV:** Outputs the cleaned and enriched data to `package-lock.csv`.

---

## Prerequisites

Before running the script, ensure you have:

1. **Python 3.x** installed.
2. Required libraries:
   ```bash
   pip install pandas requests
   ```
   
## How It Works

### 1. Load the JSON File  
The script reads the `package-lock.json` file and parses it into a Pandas DataFrame for easy manipulation.

### 2. Clean and Prepare Data  
- **Renames columns** for better readability:
  - `name` → `Project`
  - `version` → `Current Version`
  - `dependencies` → `dict`
- **Adds new columns** to store additional information:
  - `Latest Version`
  - `Integrity`
  - `Requires`
  - `Dependencies`
  - `hasDependencies` (boolean flag indicating whether a dependency has its own dependencies)

### 3. Process Dependency Information  
Extracts and processes data from the `dependencies` dictionary:
- **Current Version:** Retrieves the version of each package.
- **Integrity Hash:** Captures the package's integrity value if available.
- **Nested Dependencies:** Lists any nested dependencies along with their versions.
- **Required Versions:** Lists required versions of dependencies.

### 4. Query NPM Registry  
For each package, the script queries the NPM registry API (`https://registry.npmjs.org/:package`) to fetch the latest available version and populates the `Latest Version` column.

### 5. Export to CSV  
The final DataFrame is saved as a CSV file named `package-lock.csv`.

---

## Usage

1. Place your `package-lock.json` file in the same directory as the script.
2. Run the script:
   ```bash
   python package_lock_to_csv.py
   ```
## Output

### Generated File
The output file, `package-lock.csv`, will be created in the same directory as the script.

### Example Output
The resulting CSV file will include the following columns:

| Packages | Project  | Current Version | Latest Version | Integrity   | Requires             | Dependencies          | hasDependencies |
|----------|----------|-----------------|----------------|-------------|----------------------|-----------------------|-----------------|
| 1        | Example1 | 1.0.0           | 1.2.0          | abc123...   | dep1: ^1.0.0, dep2:  | dep3: 1.1.0, dep4:    | True            |

---

## Notes

### Performance Considerations
- The script sends a separate request to the NPM registry for each package. This can slow down processing for projects with a large number of dependencies.
- **Optimization Suggestions:**
  - Use bulk queries if supported by the API.
  - Implement caching for frequently queried packages.

### Warning Suppression
- Warnings are suppressed in the script for a cleaner execution log.

---

## Limitations

1. The script assumes a specific structure for the `package-lock.json` file.
2. An active internet connection is required to fetch the latest package versions.

---

## License

This project is open-source and available under the MIT License.


