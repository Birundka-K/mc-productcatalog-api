# MC Product Catalog Sync API

A MuleSoft Batch Processing API that synchronizes product catalog data from a CSV file into Salesforce and Snowflake. The application processes each product record using Mule Batch Processing, ensuring reliable synchronization, logging, and error handling.

---

## Features

- Read product data from CSV
- Batch Processing 
- Salesforce Product2 Upsert
- Snowflake Product Insert
- Record-level Error Handling
- Batch-level Error Handling

---

## API Endpoint

### Start Product Synchronization

```
POST /prodsync
```

No request body is required.

The application automatically reads the configured CSV file and starts the batch process.

---

## CSV Format

Example:

```csv
Id__c,ProductCode,Name
P1001,PRD001,Laptop
P1002,PRD002,Keyboard
P1003,PRD003,Mouse
```

---

## Batch Processing Flow

```
HTTP Request
      │
      ▼
Read CSV File
      │
      ▼
Transform CSV Records
      │
      ▼
Batch Job
      │
      ├──────────────────────┐
      ▼                      ▼
Batch Step 1           Batch Step 2
Salesforce             Snowflake
Product Upsert         Product Insert
      │                      │
      └──────────────┬───────┘
                     ▼
            Batch On Complete
                     │
                     ▼
          Return Success Response
```

---

## Batch Steps

### Step 1 – Salesforce Synchronization

- Upsert Product2 records
- Uses External ID (`Id__c`)
- Logs successful synchronization
- Handles Salesforce connector exceptions

---

### Step 2 – Snowflake Synchronization

- Inserts product records into Snowflake
- Logs successful insertion
- Handles database-related exceptions

---

## Batch Completion

After all records are processed, the application:

- Displays total processed records
- Displays successful records
- Displays failed records
- Logs batch completion status
- Returns a success response

---

## Error Handling

The application provides multiple levels of error handling.


- Salesforce Connectivity
- Salesforce Timeout
- Invalid Input
- Snowflake Connectivity
- SQL Execution Errors

Failed records are logged without stopping the batch job.

---


## Author

Birundha K
