# Job Search Platform

[![Java CI](https://github.com/ChoudharyUday212000/JobSearchPlatform/actions/workflows/ci.yml/badge.svg)](https://github.com/ChoudharyUday212000/JobSearchPlatform/actions/workflows/ci.yml)

A Spring Boot REST API for a job-search workflow. The codebase is organized into controller, service, model and repository packages, with MySQL persistence through Spring Data JPA.

## Features in the current codebase

- Search job listings by title, company, location, category, employment type and salary.
- Create job listings and trigger matching-category job alerts.
- Create and retrieve job applications.
- Separate modules for profiles, resumes, notifications and interview scheduling.
- MySQL-backed persistence with HikariCP connection-pool configuration.

## Tech stack

Java 17 · Spring Boot 3.4 · Spring Web · Spring Data JPA · MySQL · Maven · JUnit 5

## Architecture

~~~text
HTTP clients
    |
    v
REST Controllers
    |
    v
Service layer -----> Notification service
    |
    v
Spring Data repositories
    |
    v
MySQL
~~~

The application is a monolithic Spring Boot service with a layered package structure. It is intentionally kept simple to make the main API and persistence flow easy to follow.

## Main API examples

### Search jobs

~~~http
GET /api/jobs/search?title=Java&location=Gurugram
~~~

Optional filters are title, company, location, category, employmentType and salary.

### Create a job listing

~~~http
POST /api/jobs
Content-Type: application/json
~~~

### Create a job application

~~~http
POST /api/job-applications
Content-Type: application/json
~~~

Retrieve a single application with GET /api/job-applications/{id}, or list applications associated with a profile using GET /api/job-applications/profile/{profileId}.

See the controllers under src/main/java for the current endpoint definitions and request models.

## Run locally

Requirements: JDK 17, Maven (or the included Maven Wrapper), and MySQL 8.

1. Start a local MySQL server and create a database named jobsearchdb.
2. Set the database connection variables. Do not put credentials in source files.

Linux/macOS:

~~~bash
export DB_URL='jdbc:mysql://localhost:3306/jobsearchdb?useSSL=false&serverTimezone=UTC'
export DB_USERNAME='root'
export DB_PASSWORD='your-local-password'
./mvnw spring-boot:run
~~~

PowerShell:

~~~powershell
$env:DB_URL = 'jdbc:mysql://localhost:3306/jobsearchdb?useSSL=false&serverTimezone=UTC'
$env:DB_USERNAME = 'root'
$env:DB_PASSWORD = 'your-local-password'
.mvnw.cmd spring-boot:run
~~~

Run tests with ./mvnw test. A GitHub Actions workflow runs Maven verification against a temporary MySQL service.

## Configuration

| Variable | Purpose | Default |
| --- | --- | --- |
| DB_URL | JDBC URL | Local jobsearchdb database |
| DB_USERNAME | MySQL username | root |
| DB_PASSWORD | MySQL password | Empty; set this locally |
| JPA_DDL_AUTO | Hibernate schema strategy | update |

## Current limitations and next steps

This repository is a portfolio project, not a production-ready job marketplace. The current code does not provide authentication/authorization or request rate limiting. Before production use, add DTOs and validation, global exception handling, pagination, database migrations, proper ownership checks for profile-specific data, structured logging, and integration tests. Avoid exposing profile or application endpoints before access control is implemented.
