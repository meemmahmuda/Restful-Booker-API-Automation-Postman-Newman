# Restful-Booker API Automation (Postman + Newman)

Automated API test suite for the [Restful-Booker](https://restful-booker.herokuapp.com) demo API, built with **Postman** and executed from the command line with **Newman**. Each run produces a detailed HTML report.

## Overview

| Item | Details |
|------|---------|
| Collection | `My_Collection` |
| Environment | `Automate_Environment` |
| API under test | https://restful-booker.herokuapp.com |
| Runner | Newman |
| Reporter | newman-reporter-htmlextra |

## Endpoints Covered

| # | Request | Method | Endpoint | Purpose |
|---|---------|--------|----------|---------|
| 1 | GetBookingIds | GET | `/booking` | List all booking IDs |
| 2 | CreateBooking | POST | `/booking` | Create a new booking |
| 3 | GetBooking | GET | `/booking/id` | Fetch a single booking |
| 4 | CreateToken | POST | `/auth` | Generate an auth token |
| 5 | UpdateBooking | PUT | `/booking/id` | Full update of a booking |
| 6 | PartialUpdateBooking | PATCH | `/booking/id` | Partial update of a booking |
| 7 | DeleteBooking | DELETE | `/booking/id` | Delete a booking |
| 8 | HealthCheck | GET | `/ping` | API health check |

## What the Tests Validate

- HTTP status codes
- Response time and response size thresholds
- JSON response format
- Data types (first name is a string, total price is a number, deposit paid is a boolean)
- Response values match the values stored in the environment (first name, last name, total price, dates, additional needs)
- Auth token is returned correctly
- Delete confirmation message

## Prerequisites

- [Node.js](https://nodejs.org/) (LTS recommended)
- Newman and the HTML reporter:

```bash
npm install -g newman newman-reporter-htmlextra
```

## Project Structure

```
.
│    My_Collection.postman_collection.json
│    Automate_Environment.postman_environment.json
├    newman/
│    └── My_Collection-2026-10-22-08-58-56-462-0.html
└──  README.md
```


## How to Run

```bash
newman run My_Collection.postman_collection.json -e Automate_Environment.postman_environment.json -r cli,htmlextra
```

Then open `newman/My_Collection-2026-10-22-08-58-56-462-0.html` in your browser.

## Sample Report Summary

Latest run: **22 October 2026**

| Metric | Result |
|--------|--------|
| Iterations | 1 |
| Total requests | 8 (0 failed) |
| Total assertions | 65 |
| Failed assertions | 7 |
| Skipped tests | 0 |
| Total run duration | 8.6s |
| Average response time | 955ms |
| Total data received | 53.09KB |

### Pass rate per request

| Request | Pass rate |
|---------|-----------|
| GetBookingIds | 75% |
| CreateBooking | 93% |
| GetBooking | 100% |
| CreateToken | 75% |
| UpdateBooking | 100% |
| PartialUpdateBooking | 100% |
| DeleteBooking | 100% |
| HealthCheck | 100% |

### Known Failures

| Request | Failed test | Reason |
|---------|-------------|--------|
| GetBookingIds | Response time below 500ms | Got 3962 ms |
| GetBookingIds | Response size below 1000B | Got 53611 B |
| CreateBooking | Status code is 201 | API returned `200` |
| GetBooking	  | Response time below 500ms	| Got 620 ms
| CreateToken | Status code is 201 | API returned `200` |
| CreateToken	| Response time below 500ms	| Got 733 ms |
| UpdateBooking	| Response time below 500ms |	Got 684 ms |

Analysis: Most failures are performance thresholds (the public Heroku-hosted API is slow) plus two status-code expectations. The API actually returns 200 for POST /booking and POST /auth, so those assertions should be updated to expect 200. The response-size limit for GetBookingIds should be raised or removed, since the endpoint returns all booking IDs.

## Report

The full interactive HTML report (request/response headers, bodies, and per-test results) is available in the 
 [**Download Newman HTML Report**](./newman) folder. Download it and open it locally in a browser to view it.

## Tech Stack

- Postman
- Newman
- newman-reporter-htmlextra
- Node.js

## Author

**meemmahmuda** — [GitHub](https://github.com/meemmahmuda)
