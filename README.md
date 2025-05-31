# Reports CRUD API Documentation

A comprehensive Firebase Cloud Functions API for managing IoT device reports with advanced querying, analytics, and bulk operations.

## Table of Contents

- [Overview](#overview)
- [Base URL](#base-url)
- [Authentication](#authentication)
- [Data Model](#data-model)
- [Core CRUD Operations](#core-crud-operations)
- [Advanced Endpoints](#advanced-endpoints)
- [Error Handling](#error-handling)
- [Testing](#testing)
- [Deployment](#deployment)

## Overview

This API provides a complete solution for managing IoT device sensor reports with features including:

- ✅ Full CRUD operations (Create, Read, Update, Delete)
- 🔍 Advanced search and filtering
- 📊 Analytics and statistics
- 🚀 Bulk operations for efficiency
- 📤 Data export (JSON/CSV)
- 📱 Real-time device monitoring
- 🔒 Input validation and error handling

## Base URL

```
https://asia-south1-qcvation.cloudfunctions.net
```

## Data Model

### Report Structure

```typescript
interface Report {
  // Required fields
  device_id: string;           // Unique device identifier
  data_type: string;           // Type of data (e.g., "sensor", "actuator")
  vendor_id: string;           // Vendor/manufacturer ID
  device_name: string;         // Human-readable device name
  data: object;                // Sensor data payload (required)
  
  // Optional fields
  device_type?: string;        // Device category
  device_model?: string;       // Device model number
  device_serial_number?: string; // Serial number
  device_firmware_version?: string; // Firmware version
  device_hardware_version?: string; // Hardware version
  
  // Auto-generated fields
  created_at: string;          // ISO timestamp
  updated_at: string;          // ISO timestamp
}
```

### Sample Report Data

```json
{
  "device_id": "TEMP_SENSOR_001",
  "data_type": "sensor",
  "vendor_id": "ACME_CORP",
  "device_name": "Temperature Sensor #1",
  "device_type": "temperature_sensor",
  "device_model": "TS-2024-PRO",
  "device_serial_number": "SN123456789",
  "device_firmware_version": "2.1.0",
  "device_hardware_version": "1.5.0",
  "data": {
        "result": [
            [
                [
                    83,
                    55,
                    138
                ],
                [
                    27,
                    37,
                    64
                ],
                [
                    86,
                    57,
                    143
                ]
            ]
        ],
        "mean": [
            [
                65.33,
                49.67,
                115.0
            ]
        ],
        "standard deviation": [
            [
                27.13,
                8.99,
                36.12
            ]
        ],
        "variance": [
            [
                41.53,
                18.1,
                31.41
            ]
        ],
        "max": [
            [
                86,
                57,
                143
            ]
        ],
        "min": [
            [
                27,
                37,
                64
            ]
        ],
        "range": [
            [
                59,
                20,
                79
            ]
        ]
    },
  "created_at": "2024-01-15T10:30:15.123Z",
  "updated_at": "2024-01-15T10:30:15.123Z"
}
```

---

## Core CRUD Operations

### 1. Create Report

Create a new device report.

**Endpoint:** `POST /createReport`

**Headers:**
```
Content-Type: application/json
```

**Request Body:**
```json
{
  "device_id": "TEMP_SENSOR_001",
  "data_type": "sensor",
  "vendor_id": "ACME_CORP",
  "device_name": "Temperature Sensor #1",
  "device_type": "temperature_sensor",
  "device_model": "TS-2024-PRO",
  "device_serial_number": "SN123456789",
  "device_firmware_version": "2.1.0",
  "device_hardware_version": "1.5.0",
  "data": {
        "result": [
            [
                [
                    83,
                    55,
                    138
                ],
                [
                    27,
                    37,
                    64
                ],
                [
                    86,
                    57,
                    143
                ]
            ]
        ],
        "mean": [
            [
                65.33,
                49.67,
                115.0
            ]
        ],
        "standard deviation": [
            [
                27.13,
                8.99,
                36.12
            ]
        ],
        "variance": [
            [
                41.53,
                18.1,
                31.41
            ]
        ],
        "max": [
            [
                86,
                57,
                143
            ]
        ],
        "min": [
            [
                27,
                37,
                64
            ]
        ],
        "range": [
            [
                59,
                20,
                79
            ]
        ]
    }
}
```

**Response (201 Created):**
```json
{
  "message": "Report created successfully",
  "id": "abc123def456",
  "data": {
    "device_id": "TEMP_SENSOR_001",
    "data_type": "sensor",
    "vendor_id": "ACME_CORP",
    "device_name": "Temperature Sensor #1",
    "created_at": "2024-01-15T10:30:15.123Z",
    "updated_at": "2024-01-15T10:30:15.123Z"
  }
}
```

### 2. Get Reports

Retrieve reports with optional pagination.

**Endpoint:** `GET /getReports`

**Query Parameters:**
- `id` (optional): Get specific report by ID
- `limit` (optional): Number of reports (default: 10, max: 100)
- `offset` (optional): Number to skip (default: 0)

**Examples:**

Get all reports:
```
GET /getReports?limit=20&offset=0
```

Get specific report:
```
GET /getReports?id=abc123def456
```

**Response (200 OK) - All Reports:**
```json
{
  "reports": [
    {
      "id": "abc123def456",
      "data": {
        "device_id": "TEMP_SENSOR_001",
        "device_name": "Temperature Sensor #1",
        "created_at": "2024-01-15T10:30:15.123Z"
      }
    }
  ],
  "count": 1,
  "limit": 20,
  "offset": 0
}
```

**Response (200 OK) - Specific Report:**
```json
{
  "id": "abc123def456",
  "data": {
    "device_id": "TEMP_SENSOR_001",
    "device_name": "Temperature Sensor #1",
    "data": {
      "temperature": 23.5,
      "humidity": 65.2
    },
    "created_at": "2024-01-15T10:30:15.123Z",
    "updated_at": "2024-01-15T10:30:15.123Z"
  }
}
```

### 3. Update Report

Update an existing report.

**Endpoint:** `PUT /updateReport?id={reportId}`

**Headers:**
```
Content-Type: application/json
```

**Query Parameters:**
- `id` (required): Report ID to update

**Request Body:**
```json
{
  "device_firmware_version": "2.2.0",
  "data": {
        "result": [
            [
                [
                    83,
                    55,
                    138
                ],
                [
                    27,
                    37,
                    64
                ],
                [
                    86,
                    57,
                    143
                ]
            ]
        ],
        "mean": [
            [
                65.33,
                49.67,
                115.0
            ]
        ],
        "standard deviation": [
            [
                27.13,
                8.99,
                36.12
            ]
        ],
        "variance": [
            [
                41.53,
                18.1,
                31.41
            ]
        ],
        "max": [
            [
                86,
                57,
                143
            ]
        ],
        "min": [
            [
                27,
                37,
                64
            ]
        ],
        "range": [
            [
                59,
                20,
                79
            ]
        ]
    }
}
```

**Response (200 OK):**
```json
{
  "message": "Report updated successfully",
  "id": "abc123def456",
  "data": {
    "device_id": "TEMP_SENSOR_001",
    "device_firmware_version": "2.2.0",
    "data": {
        "result": [
            [
                [
                    83,
                    55,
                    138
                ],
                [
                    27,
                    37,
                    64
                ],
                [
                    86,
                    57,
                    143
                ]
            ]
        ],
        "mean": [
            [
                65.33,
                49.67,
                115.0
            ]
        ],
        "standard deviation": [
            [
                27.13,
                8.99,
                36.12
            ]
        ],
        "variance": [
            [
                41.53,
                18.1,
                31.41
            ]
        ],
        "max": [
            [
                86,
                57,
                143
            ]
        ],
        "min": [
            [
                27,
                37,
                64
            ]
        ],
        "range": [
            [
                59,
                20,
                79
            ]
        ]
    },
    "created_at": "2024-01-15T10:30:15.123Z",
    "updated_at": "2024-01-15T11:45:22.456Z"
  }
}
```

### 4. Delete Report

Delete a report by ID.

**Endpoint:** `DELETE /deleteReport?id={reportId}`

**Query Parameters:**
- `id` (required): Report ID to delete

**Response (200 OK):**
```json
{
  "message": "Report deleted successfully",
  "id": "abc123def456"
}
```

### 5. Search Reports

Search reports with basic filters.

**Endpoint:** `GET /searchReports`

**Query Parameters:**
- `device_id` (optional): Filter by device ID
- `data_type` (optional): Filter by data type
- `vendor_id` (optional): Filter by vendor ID
- `limit` (optional): Number of results (default: 10)

**Example:**
```
GET /searchReports?device_id=TEMP_SENSOR_001&data_type=sensor&limit=5
```

**Response (200 OK):**
```json
{
  "reports": [
    {
      "id": "abc123def456",
      "data": {
        "device_id": "TEMP_SENSOR_001",
        "data_type": "sensor",
        "vendor_id": "ACME_CORP"
      }
    }
  ],
  "count": 1,
  "filters": {
    "device_id": "TEMP_SENSOR_001",
    "data_type": "sensor",
    "vendor_id": null
  }
}
```

---

## Advanced Endpoints

### 6. Get Reports by Device ID

Advanced device-specific querying with date filtering and sorting.

**Endpoint:** `GET /getReportsByDeviceId`

**Query Parameters:**
- `device_id` (required): Device ID to filter by
- `limit` (optional): Number of results (default: 10)
- `offset` (optional): Number to skip (default: 0)
- `start_date` (optional): ISO timestamp (inclusive)
- `end_date` (optional): ISO timestamp (inclusive)
- `sort_by` (optional): Field to sort by (default: "created_at")
- `sort_order` (optional): "asc" or "desc" (default: "desc")

**Example:**
```
GET /getReportsByDeviceId?device_id=TEMP_SENSOR_001&start_date=2024-01-01T00:00:00.000Z&end_date=2024-01-31T23:59:59.999Z&limit=50&sort_by=updated_at&sort_order=desc
```

**Response (200 OK):**
```json
{
  "reports": [
    {
      "id": "abc123def456",
      "data": {
        "device_id": "TEMP_SENSOR_001",
        "created_at": "2024-01-15T10:30:15.123Z"
      }
    }
  ],
  "count": 1,
  "device_id": "TEMP_SENSOR_001",
  "pagination": {
    "limit": 50,
    "offset": 0,
    "has_more": false
  },
  "filters": {
    "start_date": "2024-01-01T00:00:00.000Z",
    "end_date": "2024-01-31T23:59:59.999Z",
    "sort_by": "updated_at",
    "sort_order": "desc"
  }
}
```

### 7. Bulk Create Reports

Create multiple reports in a single operation.

**Endpoint:** `POST /bulkCreateReports`

**Headers:**
```
Content-Type: application/json
```

**Limits:**
- Maximum 100 reports per request
- All reports must pass validation

**Request Body:**
```json
[
  {
    "device_id": "SENSOR_001",
    "data_type": "sensor",
    "vendor_id": "ACME_CORP",
    "device_name": "Sensor 1",
    "data": {"temperature": 23.5}
  },
  {
    "device_id": "SENSOR_002",
    "data_type": "sensor",
    "vendor_id": "ACME_CORP",
    "device_name": "Sensor 2",
    "data": {"temperature": 24.1}
  }
]
```

**Response (201 Created):**
```json
{
  "message": "Reports created successfully",
  "created_count": 2,
  "results": [
    {
      "index": 0,
      "id": "report_id_1",
      "status": "queued"
    },
    {
      "index": 1,
      "id": "report_id_2",
      "status": "queued"
    }
  ]
}
```

### 8. Bulk Delete Reports

Delete multiple reports by their IDs.

**Endpoint:** `DELETE /bulkDeleteReports`

**Headers:**
```
Content-Type: application/json
```

**Limits:**
- Maximum 100 report IDs per request

**Request Body:**
```json
{
  "reportIds": ["report_id_1", "report_id_2", "report_id_3"]
}
```

**Response (200 OK):**
```json
{
  "message": "Bulk delete completed",
  "deleted_count": 2,
  "not_found_count": 1,
  "results": [
    {
      "id": "report_id_1",
      "status": "deleted"
    },
    {
      "id": "report_id_2",
      "status": "deleted"
    },
    {
      "id": "report_id_3",
      "status": "not_found"
    }
  ]
}
```

### 9. Get Reports Analytics

Comprehensive analytics and statistics.

**Endpoint:** `GET /getReportsAnalytics`

**Query Parameters:**
- `device_id` (optional): Filter by device ID
- `vendor_id` (optional): Filter by vendor ID
- `start_date` (optional): Start date for analysis
- `end_date` (optional): End date for analysis

**Example:**
```
GET /getReportsAnalytics?start_date=2024-01-01T00:00:00.000Z&end_date=2024-01-31T23:59:59.999Z
```

**Response (200 OK):**
```json
{
  "total_reports": 1250,
  "date_range": {
    "start_date": "2024-01-01T00:00:00.000Z",
    "end_date": "2024-01-31T23:59:59.999Z"
  },
  "device_counts": {
    "TEMP_SENSOR_001": 45,
    "TEMP_SENSOR_002": 38,
    "HUMIDITY_SENSOR_001": 67
  },
  "vendor_counts": {
    "ACME_CORP": 890,
    "TECH_SOLUTIONS": 360
  },
  "data_type_counts": {
    "sensor": 1100,
    "actuator": 150
  },
  "daily_counts": {
    "2024-01-01": 35,
    "2024-01-02": 42,
    "2024-01-03": 38
  },
  "top_devices": [
    ["HUMIDITY_SENSOR_001", 67],
    ["TEMP_SENSOR_001", 45],
    ["TEMP_SENSOR_002", 38]
  ],
  "top_vendors": [
    ["ACME_CORP", 890],
    ["TECH_SOLUTIONS", 360]
  ]
}
```

### 10. Advanced Search Reports

Complex multi-criteria search with flexible filtering.

**Endpoint:** `POST /advancedSearchReports`

**Headers:**
```
Content-Type: application/json
```

**Query Parameters:**
- `limit` (optional): Number of results (default: 10)
- `offset` (optional): Number to skip (default: 0)

**Request Body:**
```json
{
  "device_ids": ["TEMP_SENSOR_001", "TEMP_SENSOR_002"],
  "vendor_ids": ["ACME_CORP"],
  "data_types": ["sensor", "actuator"],
  "device_name_contains": "temp",
  "created_after": "2024-01-01T00:00:00.000Z",
  "created_before": "2024-01-31T23:59:59.999Z",
  "sort_by": "created_at",
  "sort_order": "desc"
}
```

**Response (200 OK):**
```json
{
  "reports": [
    {
      "id": "abc123def456",
      "data": {
        "device_id": "TEMP_SENSOR_001",
        "device_name": "Temperature Sensor #1",
        "created_at": "2024-01-15T10:30:15.123Z"
      }
    }
  ],
  "count": 1,
  "search_criteria": {
    "device_ids": ["TEMP_SENSOR_001", "TEMP_SENSOR_002"],
    "vendor_ids": ["ACME_CORP"],
    "data_types": ["sensor", "actuator"],
    "device_name_contains": "temp",
    "created_after": "2024-01-01T00:00:00.000Z",
    "created_before": "2024-01-31T23:59:59.999Z",
    "sort_by": "created_at",
    "sort_order": "desc"
  },
  "pagination": {
    "limit": 10,
    "offset": 0,
    "has_more": false
  }
}
```

### 11. Get Latest Reports by Device

Get the most recent report for each unique device.

**Endpoint:** `GET /getLatestReportsByDevice`

**Query Parameters:**
- `limit` (optional): Maximum number of devices (default: 50)

**Example:**
```
GET /getLatestReportsByDevice?limit=100
```

**Response (200 OK):**
```json
{
  "reports": [
    {
      "id": "latest_report_1",
      "data": {
        "device_id": "TEMP_SENSOR_001",
        "device_name": "Temperature Sensor #1",
        "data": {
          "temperature": 23.5,
          "battery_level": 87
        },
        "created_at": "2024-01-15T10:30:15.123Z"
      }
    },
    {
      "id": "latest_report_2",
      "data": {
        "device_id": "HUMIDITY_SENSOR_001",
        "device_name": "Humidity Sensor #1",
        "data": {
          "humidity": 65.2,
          "battery_level": 92
        },
        "created_at": "2024-01-15T09:45:30.789Z"
      }
    }
  ],
  "count": 2,
  "unique_devices": 2
}
```

### 12. Export Reports

Export reports in JSON or CSV format.

**Endpoint:** `GET /exportReports`

**Query Parameters:**
- `format` (optional): "json" or "csv" (default: "json")
- `device_id` (optional): Filter by device ID
- `start_date` (optional): Start date filter
- `end_date` (optional): End date filter
- `limit` (optional): Maximum records (default: 1000, max: 10000)

**JSON Export Example:**
```
GET /exportReports?format=json&device_id=TEMP_SENSOR_001&limit=500
```

**JSON Response (200 OK):**
```json
{
  "reports": [
    {
      "id": "abc123def456",
      "device_id": "TEMP_SENSOR_001",
      "device_name": "Temperature Sensor #1",
      "vendor_id": "ACME_CORP",
      "data_type": "sensor",
      "created_at": "2024-01-15T10:30:15.123Z",
      "updated_at": "2024-01-15T10:30:15.123Z"
    }
  ],
  "count": 1,
  "export_date": "2024-01-15T12:00:00.000Z",
  "filters": {
    "device_id": "TEMP_SENSOR_001",
    "start_date": null,
    "end_date": null,
    "limit": 500
  }
}
```

**CSV Export Example:**
```
GET /exportReports?format=csv&start_date=2024-01-01T00:00:00.000Z&limit=1000
```

**CSV Response (200 OK):**
```
Content-Type: text/csv
Content-Disposition: attachment; filename="reports_1705320000000.csv"

id,device_id,device_name,vendor_id,data_type,device_type,device_model,device_serial_number,device_firmware_version,device_hardware_version,created_at,updated_at
"abc123def456","TEMP_SENSOR_001","Temperature Sensor #1","ACME_CORP","sensor","temperature_sensor","TS-2024-PRO","SN123456789","2.1.0","1.5.0","2024-01-15T10:30:15.123Z","2024-01-15T10:30:15.123Z"
```

---

## Error Handling

### Standard Error Responses

**400 Bad Request:**
```json
{
  "error": "Missing required fields",
  "missingFields": ["device_id", "data_type"]
}
```

**404 Not Found:**
```json
{
  "error": "Report not found",
  "id": "invalid-report-id"
}
```

**405 Method Not Allowed:**
```
Method Not Allowed
```

**500 Internal Server Error:**
```
Internal Server Error: Could not create report.
```

### Validation Rules

- **Required Fields:** `device_id`, `data_type`, `vendor_id`, `device_name`, `data`
- **Field Types:** All fields must match expected data types
- **Bulk Limits:** Maximum 100 items per bulk operation
- **Export Limits:** Maximum 10,000 records per export
- **Pagination:** Maximum 100 items per page

---

## Testing

### Comprehensive Test Suite

The project includes 10 comprehensive test cases covering:

1. ✅ **Create Report with Valid Data** - Tests successful report creation
2. ✅ **Create Report with Missing Required Fields** - Tests validation
3. ✅ **Create Report with Wrong Content-Type** - Tests content-type validation
4. ✅ **Get Specific Report by ID** - Tests report retrieval
5. ✅ **Get Non-existent Report** - Tests 404 error handling
6. ✅ **Get All Reports with Pagination** - Tests listing with pagination
7. ✅ **Update Existing Report** - Tests report updates
8. ✅ **Update Non-existent Report** - Tests update error handling
9. ✅ **Search Reports by Device ID** - Tests search functionality
10. ✅ **Delete Report** - Tests deletion and verification

### Running Tests

#### Option 1: Node.js Test Runner (Recommended)
```bash
# Run with default URL
node sample/run-tests.js

# Run with custom URL
node sample/run-tests.js https://asia-south1-qcvation.cloudfunctions.net
```

#### Option 2: Browser/Fetch API Tests
```javascript
// In sample/test-cases.js
const BASE_URL = 'https://asia-south1-qcvation.cloudfunctions.net';

// Run all tests
runAllTests();

// Or run individual tests
await testCase1_CreateValidReport();
```

#### Option 3: Simple Test Script
```javascript
// In sample/test-crud-api.js
const BASE_URL = 'https://asia-south1-qcvation.cloudfunctions.net';
runTests();
```

### Test Output Example
```
🧪 Starting Comprehensive CRUD API Tests

=== Test 1: Create Report with Valid Data ===
✅ PASS: Valid report created successfully

=== Test 2: Create Report with Missing Required Fields ===
✅ PASS: Validation correctly rejected invalid report

...

📊 Test Results: 10/10 tests passed
⏱️  Total execution time: 3.45s
🎉 All tests passed! API is working correctly.
```

---

## Deployment

### Prerequisites

1. Firebase CLI installed and configured
2. Firebase project with Firestore enabled
3. Node.js 18+ environment

### Deploy Functions

```bash
# Navigate to functions directory
cd functions

# Install dependencies
npm install

# Build TypeScript
npm run build

# Deploy to Firebase
npm run deploy

# Or deploy specific function
firebase deploy --only functions:createReport
```

### Environment Setup

1. **Initialize Firebase Project:**
   ```bash
   firebase init functions
   ```

2. **Configure Firestore Security Rules:**
   ```javascript
   // firestore.rules
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /reports/{document} {
         allow read, write: if request.auth != null;
       }
     }
   }
   ```

3. **Set Environment Variables:**
   ```bash
   firebase functions:config:set api.cors=true
   firebase functions:config:set api.region="asia-south1"
   ```

### Production Considerations

- **Indexing:** Create Firestore composite indexes for complex queries
- **Security:** Implement proper authentication and authorization
- **Rate Limiting:** Add rate limiting for production use
- **Monitoring:** Set up logging and monitoring
- **Backup:** Configure automated backups for Firestore

---

## API Summary

| Endpoint | Method | Purpose | Key Features |
|----------|--------|---------|--------------|
| `/createReport` | POST | Create single report | Validation, timestamps |
| `/getReports` | GET | Get reports | Pagination, single/multiple |
| `/updateReport` | PUT | Update report | Partial updates, timestamps |
| `/deleteReport` | DELETE | Delete report | ID validation |
| `/searchReports` | GET | Basic search | Device/vendor/type filters |
| `/getReportsByDeviceId` | GET | Device-specific | Date range, sorting |
| `/bulkCreateReports` | POST | Bulk create | Batch processing, validation |
| `/bulkDeleteReports` | DELETE | Bulk delete | Status tracking |
| `/getReportsAnalytics` | GET | Analytics | Statistics, trends |
| `/advancedSearchReports` | POST | Complex search | Multi-criteria filtering |
| `/getLatestReportsByDevice` | GET | Latest per device | Unique device reports |
| `/exportReports` | GET | Data export | JSON/CSV formats |

---

**Version:** 1.0.0  
**Last Updated:** January 2024  
**Support:** [GitHub Issues](https://github.com/your-repo/issues) 
