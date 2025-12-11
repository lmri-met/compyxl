# `compyxl`

## 📊 Robust Numerical Validation for Complex Excel Models

The **`compyxl`** library provides a scalable and maintainable framework for numerically validating complex Excel workbooks against replicated Python logic. By leveraging Object-Oriented Programming (OOP), it allows developers to define a single, standard validation workflow that is reused across multiple, specialized Excel models, ensuring data integrity and reconciliation between Excel's output and reliable Python calculations.

## ✨ Features

  * **Inheritance-Based Validation:** Define a generic workflow once, and specialize the configuration (sheet names, columns, formulas) for each unique Excel file.
  * **Floating-Point Robustness:** Built-in tolerance checking ensures differences due to precision errors do not trigger false failures.
  * **Clear Reporting:** Provides a detailed report of discrepancies, including input values, Excel output, Python output, and the exact difference.
  * **Modular Design:** Easy to extend and test due to the separation of core logic from file-specific configuration.

## 🛠️ Installation

This library is designed to be installed as a local package within your environment.

### Prerequisites

  * Python 3.8+
  * The required dependencies listed in `requirements.txt` (e.g., `pandas`, `openpyxl`, `numpy`).

### Steps

1.  Clone the repository:

    ```bash
    git clone https://github.com/your-username/ComPyXL.git
    cd ComPyXL
    ```

2.  (Optional but Recommended) Create and activate a virtual environment:

    ```bash
    python -m venv venv
    source venv/bin/activate # On Windows, use: venv\Scripts\activate
    ```

3.  Install the dependencies:

    ```bash
    pip install -r requirements.txt
    ```


## 🚀 Quick Start & Usage

To validate a file, you must first create a **subclass** that defines the specific configuration and logic for that Excel model.

### 1\. Define Your Custom Formula

In a file (e.g., `src/projects/my_new_model.py`), define the exact Python function that replicates the Excel logic.

```python
# src/projects/my_new_model.py

import pandas as pd
import numpy as np

def calculate_net_margin(revenue: pd.Series, cost: pd.Series, tax_rate: pd.Series) -> pd.Series:
    """Replicates Excel: (Revenue - Cost) * (1 - Tax_Rate)"""
    return (revenue - cost) * (1 - tax_rate)
```

### 2\. Create the Validator Subclass

Create a subclass that inherits from `BaseExcelValidator` and overrides the essential configuration method:

```python
# src/projects/my_new_model.py

from src.core.base_validator import BaseExcelValidator
from .my_formula import calculate_net_margin # Assuming formula is imported

class ProjectXValidator(BaseExcelValidator):
    """Validator for the complex 'Project_X_Workbook.xlsx'"""

    def _define_config(self):
        """Sets all file-specific parameters."""
        self.sheet_name = "Final_Summary_Sheet"
        self.input_cols = ["Revenue", "Cost", "Tax_Rate"]
        self.excel_result_col = "Net_Margin_Excel"
        self.formula_func = calculate_net_margin
        # self.tolerance = 1e-12 # Optional: Set custom tolerance
```

### 3\. Execute the Validation

You can now run the validation script from a main execution file:

```python
# run_validation.py

# Import the class from the compyxl library structure
from compyxl.projects.my_new_model import ProjectXValidator 

# Path provided by the user/system
FILE_TO_VALIDATE = 'C:/Data/Project_X_Workbook.xlsx'

# Instantiate and run
validator = ProjectXValidator(file_path=FILE_TO_VALIDATE)
validator.validate()
```

#### Example Output (Failure)

```
--- Starting Validation for: C:/Data/Project_X_Workbook.xlsx on sheet 'Final_Summary_Sheet' ---
-> Read sheet 'Final_Summary_Sheet'. Rows: 500
-> Python computation complete.

❌ FAILED: Task C:/Data/Project_X_Workbook.xlsx [Final_Summary_Sheet]. Found 3 discrepancies.

--- Discrepancy Report Snippet ---
       Revenue      Cost  Tax_Rate  Net_Margin_Excel  Python_Result  Difference
10  10000.0000  5000.000      0.25           3750.00      3749.9999    0.000001
25   5500.0000  1200.000      0.15           3655.00      3654.8000    0.200000
48   9000.0000  7000.000      0.00           2000.00      1999.9999    0.000001
-----------------------------------
```


## 📚 Documentation & Architecture

For in-depth details on the library's structure and policies, please refer to the following documents in the `docs/` directory:

  * [`architecture.md`](https://www.google.com/search?q=docs/architecture.md): High-Level Architecture Document (HAD)
  * [`api_spec.md`](https://www.google.com/search?q=docs/api_spec.md): Interface Specification Document (API Definition)
  * [`functional_spec.md`](https://www.google.com/search?q=docs/functional_spec.md): Functional Specification Document (Tolerance Policy, Error Handling)


## 🤝 Contributing

Contributions are welcome\! Please refer to the documentation for guidelines on adding new features or fixing bugs.
