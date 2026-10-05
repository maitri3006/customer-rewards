# Customer Rewards

Spring Boot REST API for calculating customer reward points.

## Technology

- Java 8
- Spring Boot 2.7.18
- Spring Data JPA
- H2 in-memory database
- Maven
- JUnit 5

Spring Boot 2.7 is used because the application needs to run on Java 8.

## Reward calculation

For each transaction:

- Less than $50: 0 points
- $50 to $100: 1 point for every dollar over $50
- More than $100: 50 points for the $50-$100 range, plus 2 points for every dollar over $100

For example, a $120 purchase earns 90 points.

## API

```text
GET /api/v1/customers/{customerId}/rewards?startDate=yyyy-MM-dd&endDate=yyyy-MM-dd
```

Example:

```text
GET http://localhost:8080/api/v1/customers/1/rewards?startDate=2025-01-01&endDate=2025-03-31
```

The response contains the customer, requested date range, monthly reward points, total reward points and the transactions used in the calculation.

## Run

Requirements:

- JDK 8
- Maven 3.6+

No database installation is required. H2 runs in memory.

```bash
mvn clean test
mvn spring-boot:run
```

The H2 console is available at:

```text
http://localhost:8080/h2-console
```

Use:

```text
JDBC URL: jdbc:h2:mem:rewardsdb
User Name: sa
Password: [leave blank]
```

## Sample data

The application creates a few customers and transactions when it starts. Alice Johnson has transactions in January, February and March 2025 for the three-month example.

## Tests

The test suite covers reward calculation boundaries, a successful REST request, an unknown customer and an invalid date range.

Run:

```bash
mvn clean test
```

## Project structure

```text
src/main/java/com/example/rewards
  config
  controller
  dto
  entity
  exception
  repository
  service

src/main/resources
  application.properties

src/test/java
  controller
  service
```

The transaction lookup is wrapped in an asynchronous service call to simulate the asynchronous data/API call requested in the assignment.
