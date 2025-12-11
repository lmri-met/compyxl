## 1\. High-Level Architecture Document (HAD)

### 1.1 Project Overview

This library, `excel_reconciliation_lib`, provides a flexible, maintainable framework for numerically validating complex Excel workbooks against replicated Python logic. It achieves this by using Python's Object-Oriented Programming (OOP) features, specifically inheritance, to separate core validation workflow from file-specific configurations.

### 1.2 Component Diagram

The system is structured around the concept of a generalized base class that is inherited by specialized validation classes.

### 1.3 Module and Class Mapping

| File Path | Module/Class | Responsibility |
| :--- | :--- | :--- |
| `src/core/base_validator.py` | `BaseExcelValidator` | Defines the generic validation workflow (`read`, `compare`, `report`). Houses reusable methods. |
| `src/projects/__init__.py` | | Exports all specialized validators for easy import. |
| `src/projects/finance_v2.py` | `FinancialModelV2Validator` | **Subclass:** Defines file path, sheet name, column names, and the unique Python formula for the Financial Model V2. |
| `src/projects/reporting_v1.py` | `QuarterlyReportV1Validator` | **Subclass:** Defines configuration for the Quarterly Report V1 workbook. |

### 1.4 Dependencies

The library relies on the following external packages:

  * `pandas` (Core data handling, reading Excel)
  * `openpyxl` (Pandas Excel engine, ensures `.xlsx` compatibility)
  * `numpy` (Efficient numerical operations within formulas)
  * `typing` (Type hinting for clarity)
