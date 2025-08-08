# 🧑‍💼 Employee Management System using AWS S3, JSON & Snowflake

This project is a cloud-based **Employee Management System** that uses **JSON** files to store employee data and leverages **AWS S3** and **Snowflake** for efficient, scalable data processing. The project demonstrates a complete **end-to-end ETL pipeline** with integration, staging, loading, and querying of semi-structured data.

---

## 🚀 Project Flow

### ✅ Step-by-Step Implementation

1. **Prepare JSON Data**  
   - Created employee data in **JSON format** with nested and array-based structures.

2. **Upload to AWS S3**  
   - Uploaded JSON files to **S3 bucket** for cloud-based storage.

3. **Create IAM Role & Policy in AWS**  
   - Granted permissions to access S3 objects securely from Snowflake.

4. **Create External Integration in Snowflake**  
   - Used `CREATE STORAGE INTEGRATION` command to connect Snowflake with S3 securely.

5. **Define File Format**  
   - Created a file format in Snowflake to interpret the structure of JSON files using:
     ```sql
     CREATE FILE FORMAT my_json_format TYPE = 'JSON';
     ```

6. **Create External Stage**  
   - Created a stage pointing to the S3 bucket using the storage integration:
     ```sql
     CREATE STAGE my_json_stage
     URL = 's3://your-bucket-name/path'
     STORAGE_INTEGRATION = my_integration
     FILE_FORMAT = my_json_format;
     ```

7. **List Files in Stage**  
   - Verified uploaded files using:
     ```sql
     LIST @my_json_stage;
     ```

8. **Create Table with VARIANT Column**  
   - Used `VARIANT` to hold semi-structured JSON data:
     ```sql
     CREATE TABLE employee_json_raw (
         emp_data VARIANT
     );
     ```

9. **Load JSON Data into Table**  
   - Used `COPY INTO` to load data:
     ```sql
     COPY INTO employee_json_raw
     FROM @my_json_stage;
     ```

10. **Use FLATTEN Function for Nested Data**  
    - Applied `FLATTEN()` to extract fields from JSON:
      ```sql
      SELECT
          emp.value:id::STRING AS emp_id,
          emp.value:name::STRING AS emp_name,
          emp.value:department::STRING AS department
      FROM employee_json_raw,
           LATERAL FLATTEN(input => emp_data);
      ```

11. **Handled JSON Arrays**  
    - Parsed arrays inside JSON where applicable using `FLATTEN` and `LATERAL`.

---

## 🔧 Tech Stack

- **Data Format**: JSON
- **Cloud Storage**: AWS S3
- **Data Warehouse**: Snowflake
- **Languages**: SQL
- **Snowflake Concepts Used**: VARIANT, FLATTEN, COPY INTO, STAGE, FILE FORMAT, STORAGE INTEGRATION

---

## 💡 Key Concepts Demonstrated

- End-to-end data pipeline from **S3 to Snowflake**
- Handling of **semi-structured data** (JSON)
- **Role and policy** creation for secure integration
- **Staging and loading** external files
- **JSON parsing and flattening** with Snowflake SQL
- Support for **arrays** and nested fields in JSON



