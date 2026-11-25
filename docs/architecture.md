# TransitVellum — proposed AWS architecture

TransitVellum uses AWS to process large volumes of counts and GPS data, forecast demand and map the network — scaling from a shuttle fleet to a city.

## Request flow

1. Counts & GPS
2. Amazon S3
3. AWS Lambda
4. Location Service
5. SageMaker AI
6. DynamoDB
7. Planner review

## Services

### Amazon SageMaker AI

**Role:** Forecast passenger demand

Trains route- and stop-level demand models on historical counts, fares and calendar effects to forecast boardings by hour for the next two weeks.

### Amazon Location Service

**Role:** Map routes and vehicles

Snaps GPS traces to roads, geocodes stops and renders network maps, so delays and crowding can be shown exactly where they happen.

### Amazon S3

**Role:** Store counts and GPS history

Holds raw and processed passenger counts, fares and vehicle locations, partitioned by date and route, as the analytics history.

### AWS Lambda

**Role:** Process and match data

Normalises vendor data, matches trips to timetables and calculates daily reliability measures automatically.

### Amazon DynamoDB

**Role:** Serve routes, forecasts and reports

Stores routes, stops, forecasts, reliability measures and recommendations for fast dashboard access.

## Principles

- **Counts, not people** — We analyse passenger numbers, never identities.
- **Vendor-neutral** — Works with data from any farebox, counter or GPS vendor.
- **Show the evidence** — Every recommendation links to the trips behind it.

## Safeguards

- **Planners decide** — Timetable and capacity changes are recommendations until a planner approves them.
- **Passenger privacy** — Only aggregate counts are used; card numbers and identities are never stored.
- **Not a dispatch system** — TransitVellum informs planning; it doesn’t dispatch or control vehicles.
- **Transparent methods** — Reliability measures follow published definitions you can audit.

## MVP scope

We import several months of passenger counts and trip data for a set of routes, forecast demand by route and hour, and deliver reliability reports and capacity suggestions for your planners.

- Passenger count import
- Route demand forecasts
- Timetable performance analysis
- Reliability report
- Capacity suggestions
