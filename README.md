# data-quality-monitor

Certainly! Below is a Python program for a project named `data-quality-monitor`, which is an automated tool designed to monitor and ensure data quality within ETL pipelines. This is a simplified version and can be expanded based on the specific needs of a project.

```python
import pandas as pd
import logging
from datetime import datetime

# Configure logging
logging.basicConfig(filename='data_quality_monitor.log', level=logging.INFO, format='%(asctime)s - %(levelname)s - %(message)s')

class DataQualityMonitor:
    def __init__(self, dataframe):
        """
        Initialize the DataQualityMonitor with a pandas DataFrame.

        :param dataframe: pd.DataFrame - The data to be monitored.
        """
        self.dataframe = dataframe

    def check_column_completeness(self, column_name):
        """
        Check if any values in a given column are missing (null).

        :param column_name: str - The name of the column to check.
        :return: str - Result of check.
        """
        if column_name not in self.dataframe.columns:
            logging.error(f"Column '{column_name}' not found in DataFrame")
            return f"Error: Column '{column_name}' does not exist."
        
        missing_values = self.dataframe[column_name].isnull().sum()
        if missing_values > 0:
            logging.warning(f"Column '{column_name}' has {missing_values} missing values")
            return f"Warning: Column '{column_name}' has {missing_values} missing values."
        else:
            logging.info(f"Column '{column_name}' has no missing values")
            return f"Column '{column_name}' has no missing values."

    def check_unique_values(self, column_name):
        """
        Check for duplicate values in the specified column.

        :param column_name: str - The name of the column to check.
        :return: str - Result of check.
        """
        if column_name not in self.dataframe.columns:
            logging.error(f"Column '{column_name}' not found in DataFrame")
            return f"Error: Column '{column_name}' does not exist."

        duplicates = self.dataframe[column_name].duplicated().sum()
        if duplicates > 0:
            logging.warning(f"Column '{column_name}' has {duplicates} duplicate values")
            return f"Warning: Column '{column_name}' has {duplicates} duplicate values."
        else:
            logging.info(f"Column '{column_name}' has all unique values")
            return f"Column '{column_name}' has all unique values."

    def check_data_types(self):
        """
        Check for expected data types in the DataFrame.

        :return: dict - Dictionary with column names as keys and data type issues as values.
        """
        issues = {}
        for column in self.dataframe.columns:
            expected_type = type(self.dataframe[column].iloc[0])
            if not all(isinstance(val, expected_type) or pd.isnull(val) for val in self.dataframe[column]):
                issues[column] = f"Expected data type {expected_type} but found other types."
                logging.warning(f"Data type issue in column '{column}': {issues[column]}")
            else:
                logging.info(f"Column '{column}' has consistent data type {expected_type}")

        return issues if issues else "All columns have consistent data types."

# Example usage:
if __name__ == "__main__":
    # Sample data
    data = {
        'name': ['Alice', 'Bob', 'Charlie', None],
        'age': [25, 30, 35, 22],
        'email': ['alice@example.com', 'bob@example.com', 'bob@example.com', 'eve@example.com']
    }

    df = pd.DataFrame(data)
    dqm = DataQualityMonitor(df)

    # Check column completeness
    print(dqm.check_column_completeness('name'))

    # Check for unique values
    print(dqm.check_unique_values('email'))

    # Check data types
    print(dqm.check_data_types())
```

### Key Concepts in the Code:

1. **Data Completeness**: Checks for any missing values in specified columns.
2. **Uniqueness**: Ensures that the specified columns have unique values.
3. **Data Type Consistency**: Verifies that all entries in a column are of the same data type.
4. **Logging**: Logging is used extensively to provide details about any errors or warnings.
5. **Error Handling**: Checks for non-existent columns to prevent runtime errors.

This code serves as a skeleton and can be expanded further with additional functionality, such as threshold checks, integrations with data sources, and more advanced data quality metrics.