# RESTful Booker API Test Automation Framework

![Postman](https://img.shields.io/badge/Postman-FF6C37?style=for-the-badge&logo=postman&logoColor=white)
![Newman](https://img.shields.io/badge/Newman-02C68A?style=for-the-badge&logo=npm&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/GitHub_Actions-2088FF?style=for-the-badge&logo=github-actions&logoColor=white)

## 📌 Project Overview
This repository contains a fully automated, stateful API testing framework validating the backend of the [RESTful Booker API](https://restful-booker.herokuapp.com). Built with Postman and executed via Newman, this project demonstrates enterprise-grade QA practices including dynamic request chaining, data-driven testing (DDT), and secure negative testing. The test suite is fully integrated into a GitHub Actions CI/CD pipeline for headless, automated execution.

## 🚀 Core Architecture & Features
* **Dynamic Request Chaining:** Automatically extracts authentication tokens and dynamic `bookingId` parameters from server responses, injecting them as environment variables into subsequent `PUT` and `DELETE` lifecycle requests.
* **Data-Driven Testing (DDT):** Ingests test data from an external `testdata.csv` file, running parameterized iterations to dynamically generate multiple distinct bookings in a single automated run.
* **Negative & Security Testing:** Validates API security boundaries by explicitly forcing unauthorized state changes and asserting correct `403 Forbidden` HTTP response codes and error messages.
* **Headless CI/CD Execution:** Configured via `.github/workflows/newman-tests.yml` to spin up an Ubuntu runner, install Node.js/Newman, and execute the full test suite automatically upon code pushes.

## 📂 Repository Structure
* `collection.json`: The exported Postman collection containing all HTTP requests, test scripts, and pre-request scripts.
* `environment.json`: The environment variables configuration (e.g., `baseUrl`, dynamic tokens).
* `testdata.csv`: The external dataset used to parameterize the POST payloads during data-driven execution.
* `.github/workflows/newman-tests.yml`: The GitHub Actions pipeline configuration file.

## 🧪 Test Scenarios Executed
The test suite executes 15 total requests across multiple iterations, validating the following end-to-end CRUD flow:
1. **POST /auth:** Generates a secure authorization token.
2. **POST /booking:** Creates a new booking using parameterized CSV data (Happy Path).
3. **PUT /booking/:id:** Updates the newly created booking using the active token (Stateful Auth).
4. **PUT /booking/:id:** Attempts to update the booking using an invalid token to verify server rejection (Negative Test -> Expects `403 Forbidden`).
5. **DELETE /booking/:id:** Deletes the record to clean the database and ensure test environment integrity.

## 💻 Local Execution Guide

**Prerequisites:**
* [Node.js](https://nodejs.org/) installed
* Newman installed globally (`npm install -g newman`)

**Run the Framework:**
Clone the repository and execute the following command in the root directory to run the collection using the environment variables and test data:
```bash
newman run collection.json -e environment.json -d testdata.csv