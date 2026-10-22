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
├── collection/
│   └── My_Collection.postman_collection.json
├── environment/
│   └── Automate_Environment.postman_environment.json
├── reports/
│   └── My_Collection-2026-10-20-17-13-56-194-0.html
└── README.md
```

> Adjust the file names/paths above to match your repository.

## How to Run

```bash
newman run collection/My_Collection.postman_collection.json \
  -e environment/Automate_Environment.postman_environment.json \
  -r htmlextra \
  --reporter-htmlextra-export reports/report.html
```

Then open `reports/report.html` in your browser.

## Sample Report Summary

Latest run: **20 October 2026**

| Metric | Result |
|--------|--------|
| Iterations | 1 |
| Total requests | 8 (0 failed) |
| Total assertions | 61 |
| Failed assertions | 4 |
| Skipped tests | 0 |
| Total run duration | 3.7s |
| Average response time | 373ms |
| Total data received | 19.95KB |

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
| HealthCheck | No tests |

### Known Failures

| Request | Failed test | Reason |
|---------|-------------|--------|
| GetBookingIds | Response time below 500ms | Got 967ms |
| GetBookingIds | Response size below 1000B | Got 19660B (the endpoint returns all booking IDs) |
| CreateBooking | Status code is 201 | API returned `200` |
| CreateToken | Status code is 201 | API returned `200` |

These failures come from test expectations that differ from the API's actual behavior (the public demo API returns `200` for these endpoints, and the booking list is large and slow on a free Heroku host). Updating the assertions, or the thresholds, will resolve them.

## Report

The full interactive HTML report (request/response headers, bodies, and per-test results) is available in the [`reports/`](./reports) folder. Download it and open it locally in a browser to view it.

## Tech Stack

- Postman
- Newman
- newman-reporter-htmlextra
- Node.js

## Author

**Your Name** — [GitHub](https://github.com/your-username)
