## 3\. Functional Specification Document (FSD)

### 3.1 Validation Workflow

The `validate()` public method executes the following four sequential phases:

1.  **Configuration:** The subclass method `_define_config()` is executed, setting parameters like sheet name and columns.
2.  **Data Loading:** The specified sheet is loaded into a Pandas DataFrame (`self.df`). Failure to load (e.g., file not found, missing sheet) results in an immediate halt.
3.  **Calculation & Comparison:**
      * The `formula_func` is applied to the input columns, generating the `Python_Result` column in `self.df`.
      * The absolute difference between `Python_Result` and `Excel_Result_Col` is calculated.
4.  **Reporting:** Rows where the absolute difference exceeds the defined tolerance are isolated and stored in `self.discrepancies`. A final summary is printed.

### 3.2 Numerical Tolerance Policy

  * **Default Value:** The default tolerance is set to $10^{-9}$ (one billionth, or `1e-9`).
  * **Justification:** Due to the unavoidable differences in floating-point arithmetic between Excel (which uses 64-bit precision) and Python/Numpy, exact equality checks are unreliable. The $10^{-9}$ threshold ensures that negligible computational differences do not trigger a validation failure.
  * **Override:** Subclasses may override the default `tolerance` value in their constructor if a specific file requires looser (e.g., for currency/rounding models) or stricter tolerance.

### 3.3 Error and Exception Handling

| Scenario | Expected Behavior | Outcome |
| :--- | :--- | :--- |
| **File Not Found** | Captured in `_read_data`. | Validation halts; prints clear error message. |
| **Missing Column** | Checked implicitly when accessing columns in `_compute_python_result`. | Python raises a `KeyError`; validation halts; prints error trace. |
| **Formula Exception** | If the custom Python formula throws an error (e.g., division by zero), it will be handled by the base class orchestrator. | Validation halts; prints error message indicating failure in computation phase. |
| **NaN/Infinity in Data** | Handled gracefully by Pandas/Numpy operations, allowing the comparison phase to proceed (NaNs will generally result in a non-comparison and should be addressed in the formula if not expected). | No halt, but a discrepancy may be flagged if the Excel result is a number and the Python result is `NaN`. |
