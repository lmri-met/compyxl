## 2\. Interface Specification Document (API Definition)

### 2.1 Base Class: `BaseExcelValidator`

This class defines the public and protected methods available to all subclasses and consumers.

| Method/Property | Type | Access | Description |
| :--- | :--- | :--- | :--- |
| `__init__` | Constructor | Public | Requires `file_path: str` and optional `tolerance: float`. |
| `validate()` | Method | **Public** | The main orchestration method. Calls protected methods in sequence: `_define_config()`, `_read_data()`, etc. |
| `report()` | Method | **Public** | Displays the validation results (PASS/FAIL) and discrepancy report. |
| `_define_config()` | Method | **Protected** | **MUST be overridden** by subclasses. Sets the required configuration parameters. |
| `_read_data()` | Method | Protected | Loads the Excel sheet into `self.df`. |
| `discrepancies` | Property | Public | Returns the DataFrame of failed rows after validation. |

### 2.2 Subclass Implementation Requirements

Any subclass (e.g., `FinancialModelV2Validator`) inheriting from `BaseExcelValidator` **must** implement the `_define_config()` method to set the following internal attributes:

| Attribute | Type | Description |
| :--- | :--- | :--- |
| `self.sheet_name` | `str` | The name of the sheet to validate. |
| `self.input_cols` | `List[str]` | The list of column headers containing input data (e.g., `['Revenue', 'Cost']`). |
| `self.excel_result_col` | `str` | The column header containing the result calculated by Excel. |
| `self.formula_func` | `Callable` | The Python function replicating the Excel calculation (must accept Pandas Series as positional arguments). |

### 2.3 Formula Function Interface

The custom Python function provided to `self.formula_func` must adhere to the following signature:

```python
def custom_formula(input_series_1: pd.Series, input_series_2: pd.Series, ...) -> pd.Series:
    # Logic replicating the Excel formula
    return result_series
```
