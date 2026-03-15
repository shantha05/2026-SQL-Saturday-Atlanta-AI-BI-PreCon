# Data Dictionary

This document provides detailed information about all datasets used in the 2026 SQL Saturday Atlanta AI/BI PreCon workshop.

---

## Table of Contents

1. [Customers](#customers)
2. [Dealerships](#dealerships)
3. [Inventory](#inventory)
4. [Sales](#sales)
5. [Service Orders](#service-orders)
6. [Telemetry Events](#telemetry-events)
7. [Test Drives](#test-drives)

---

## Customers

**File:** `customers.csv`  
**Description:** Contains information about customers including their personal details, segmentation, location, and loyalty program data.  
**Record Count:** ~7,000 rows

| Column Name | Data Type | Description |
|------------|-----------|-------------|
| CustomerID | Integer | Unique identifier for each customer (Primary Key) |
| FirstName | String | Customer's first name |
| LastName | String | Customer's last name |
| Segment | String | Customer segment classification (e.g., Retail, Fleet, Luxury) |
| City | String | City where the customer resides |
| Region | String | Geographic region (e.g., North America, Europe, ANZ, India) |
| Country | String | Country where the customer resides |
| PreferredChannel | String | Customer's preferred communication channel (e.g., Walk-in, Email, SMS) |
| LoyaltyTier | String | Customer loyalty program tier (e.g., Gold, Silver, Platinum) |
| SignupDate | Date | Date when the customer signed up (Format: YYYY-MM-DD) |

**Sample Values:**

- Segments: Retail, Fleet, Luxury
- Regions: North America, Europe, ANZ, India
- Preferred Channels: Walk-in, Email, SMS
- Loyalty Tiers: Gold, Silver, Platinum

---

## Dealerships

**File:** `dealerships.csv`  
**Description:** Contains information about dealership locations, capacity, and facility characteristics.  
**Record Count:** ~48 rows

| Column Name | Data Type | Description |
|------------|-----------|-------------|
| DealershipID | Integer | Unique identifier for each dealership (Primary Key) |
| DealershipName | String | Name of the dealership |
| City | String | City where the dealership is located |
| Region | String | Geographic region (e.g., North America, Europe, ANZ, India) |
| Country | String | Country where the dealership is located |
| Latitude | Decimal | Geographic latitude coordinate |
| Longitude | Decimal | Geographic longitude coordinate |
| OpenYear | Integer | Year when the dealership opened |
| FranchiseTier | String | Dealership tier classification (e.g., Standard, Flagship) |
| ServiceBays | Integer | Number of service bays available |
| ShowroomCapacity | Integer | Number of vehicles that can be displayed in showroom |

**Sample Values:**

- Franchise Tiers: Standard, Flagship
- Service Bays: Typically 10-16 bays
- Showroom Capacity: Typically 40-60 vehicles

---

## Inventory

**File:** `inventory.csv`  
**Description:** Contains information about vehicle inventory including specifications, pricing, and current status.  
**Record Count:** ~6,500 rows

| Column Name | Data Type | Description |
|------------|-----------|-------------|
| VehicleID | Integer | Unique identifier for each vehicle (Primary Key) |
| VIN | String | Vehicle Identification Number (17 characters) |
| Make | String | Vehicle manufacturer (e.g., Fabrikam Auto, Contoso Motors, Northwind) |
| Model | String | Vehicle model name |
| Segment | String | Vehicle category (e.g., Sedan, SUV) |
| Year | Integer | Manufacturing year of the vehicle |
| Trim | String | Vehicle trim level (e.g., Base, Sport, Limited) |
| Color | String | Exterior color of the vehicle |
| FuelType | String | Type of fuel (e.g., Petrol, Diesel, Electric) |
| PurchaseCost | Decimal | Dealership's cost to acquire the vehicle |
| MSRP | Decimal | Manufacturer's Suggested Retail Price |
| Status | String | Current status of the vehicle (e.g., In Stock, In Transit, Sold) |
| DealershipID | Integer | Dealership where vehicle is located (Foreign Key to Dealerships) |
| DaysOnLot | Integer | Number of days the vehicle has been in inventory |

**Sample Values:**

- Makes: Fabrikam Auto, Contoso Motors, Northwind
- Segments: Sedan, SUV
- Trims: Base, Sport, Limited
- Status: In Stock, In Transit, Sold
- Fuel Types: Petrol, Diesel, Electric

---

## Sales

**File:** `sales.csv`  
**Description:** Contains transaction records for vehicle sales including pricing, financing, and lead information.  
**Record Count:** ~4,200 rows

| Column Name | Data Type | Description |
|------------|-----------|-------------|
| SaleID | Integer | Unique identifier for each sale transaction (Primary Key) |
| Date | Date | Date when the sale was completed (Format: YYYY-MM-DD) |
| VehicleID | Integer | Vehicle that was sold (Foreign Key to Inventory) |
| DealershipID | Integer | Dealership where sale occurred (Foreign Key to Dealerships) |
| CustomerID | Integer | Customer who purchased the vehicle (Foreign Key to Customers) |
| Salesperson | String | Name of the salesperson (Format: Initial. LastName) |
| SalePrice | Decimal | Final sale price of the vehicle |
| Discount | Decimal | Discount amount applied to the sale |
| FinanceType | String | Type of financing used (e.g., Bank Loan, Manufacturer Finance, Cash) |
| WarrantySold | Boolean | Whether an extended warranty was sold (True/False) |
| TradeInFlag | Boolean | Whether customer traded in a vehicle (True/False) |
| LeadSource | String | Source of the sales lead (e.g., Walk-in, Referral, Online) |

**Sample Values:**

- Finance Types: Bank Loan, Manufacturer Finance, Cash
- Lead Sources: Walk-in, Referral, Online

---

## Service Orders

**File:** `service_orders.csv`  
**Description:** Contains records of vehicle service and maintenance work performed at dealerships.  
**Record Count:** ~12,000 rows

| Column Name | Data Type | Description |
|------------|-----------|-------------|
| ServiceOrderID | Integer | Unique identifier for each service order (Primary Key) |
| VehicleID | Integer | Vehicle being serviced (Foreign Key to Inventory) |
| CustomerID | Integer | Customer who owns the vehicle (Foreign Key to Customers) |
| DealershipID | Integer | Dealership performing the service (Foreign Key to Dealerships) |
| OpenDate | Date | Date when service order was opened (Format: YYYY-MM-DD) |
| CloseDate | Date | Date when service order was completed (Format: YYYY-MM-DD) |
| ServiceType | String | Type of service performed (e.g., Body Repair, Tire Rotation, Brake Service) |
| PartsCost | Decimal | Cost of parts used in service |
| LaborCost | Decimal | Cost of labor for the service |
| TotalCost | Decimal | Total cost of the service (Parts + Labor) |
| SatisfactionScore | Decimal | Customer satisfaction rating (Scale: 1.0-5.0) |
| TechnicianLevel | String | Skill level of the technician (e.g., L1, L2, L3, Master) |

**Sample Values:**

- Service Types: Body Repair, Tire Rotation, Brake Service, Oil Change, Diagnostics
- Technician Levels: L1 (Entry), L2, L3 (Advanced), Master
- Satisfaction Score: Range from 1.0 to 5.0

---

## Telemetry Events

**File:** `telemetry_events.jsonl`  
**Description:** Contains IoT sensor and device events captured at dealership locations for customer experience monitoring.  
**Record Count:** ~50,000 rows  
**Format:** JSON Lines (one JSON object per line)

| Field Name | Data Type | Description |
|-----------|-----------|-------------|
| EventID | Integer | Unique identifier for each telemetry event (Primary Key) |
| EventTime | DateTime | Timestamp when the event occurred (Format: ISO 8601) |
| DealershipID | Integer | Dealership where event occurred (Foreign Key to Dealerships) |
| DealershipName | String | Name of the dealership |
| City | String | City where the event occurred |
| Region | String | Geographic region |
| Country | String | Country where the event occurred |
| DeviceType | String | Type of IoT device that captured the event |
| Zone | String | Zone within the dealership where event occurred |
| EventType | String | Type of event captured |
| CustomerCount | Integer | Number of customers detected during the event |
| WaitTimeSeconds | Integer | Wait time in seconds |
| SentimentScore | Decimal | Customer sentiment score (Range: -1.0 to 1.0) |
| QueueLength | Integer | Number of people in queue |
| AlertFlag | Boolean | Whether the event triggered an alert |

**Sample Values:**

- Device Types: door_sensor, camera, parking_sensor, mobile_app, kiosk
- Zones: EV Zone, Service Desk, Parking, Showroom
- Event Types: promo_screen_touch, test_drive_end, test_drive_start, showroom_entry
- Sentiment Score: Range from -1.0 (negative) to 1.0 (positive)

---

## Test Drives

**File:** `test_drives.csv`  
**Description:** Contains records of customer test drive sessions and their outcomes.  
**Record Count:** ~9,000 rows

| Column Name | Data Type | Description |
|------------|-----------|-------------|
| TestDriveID | Integer | Unique identifier for each test drive (Primary Key) |
| DateTime | DateTime | Date and time when test drive occurred (Format: YYYY-MM-DD HH:MM:SS) |
| CustomerID | Integer | Customer who took the test drive (Foreign Key to Customers) |
| VehicleID | Integer | Vehicle used for test drive (Foreign Key to Inventory) |
| DealershipID | Integer | Dealership where test drive occurred (Foreign Key to Dealerships) |
| DurationMinutes | Integer | Duration of the test drive in minutes |
| Outcome | String | Result of the test drive (e.g., Converted, No Decision, Thinking) |
| BookedVia | String | Channel used to book the test drive (e.g., Phone, App, Walk-in) |

**Sample Values:**

- Durations: Typically 20, 30, or 45 minutes
- Outcomes: Converted, No Decision, Thinking
- Booking Channels: Phone, App, Walk-in

---

## Entity Relationship Overview

### Primary Keys

- Customers: CustomerID
- Dealerships: DealershipID
- Inventory: VehicleID
- Sales: SaleID
- Service Orders: ServiceOrderID
- Telemetry Events: EventID
- Test Drives: TestDriveID

### Foreign Key Relationships

**Inventory:**

- DealershipID → Dealerships.DealershipID

**Sales:**

- VehicleID → Inventory.VehicleID
- DealershipID → Dealerships.DealershipID
- CustomerID → Customers.CustomerID

**Service Orders:**

- VehicleID → Inventory.VehicleID
- CustomerID → Customers.CustomerID
- DealershipID → Dealerships.DealershipID

**Test Drives:**

- CustomerID → Customers.CustomerID
- VehicleID → Inventory.VehicleID
- DealershipID → Dealerships.DealershipID

**Telemetry Events:**

- DealershipID → Dealerships.DealershipID

---

## Data Quality Notes

- All date fields use ISO format (YYYY-MM-DD or ISO 8601 for datetime)
- Decimal values are used for monetary amounts and precise measurements
- Boolean values are represented as True/False
- Geographic coordinates are in decimal degrees format
- VIN numbers follow standard 17-character format
- Customer and Vehicle IDs are consistent across all tables for relational integrity

---

*Last Updated: March 16, 2026*
