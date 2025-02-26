# Asynchronous Database Query Processor for AWS RDS (PostgreSQL)
## [Medium Blog Link](https://medium.com/@shashikhanal/mastering-asynchronous-database-queries-how-we-built-our-own-query-processor-for-aws-rds-6c1d03f2f6f0)

An advanced database query processor for handling asynchronous database operations, enabling non-blocking interaction with AWS RDS (Aurora PostgreSQL). This approach optimizes application performance by allowing tasks to proceed while awaiting database responses.

## Features
- Handles asynchronous `SELECT`, `INSERT`, and `UPDATE` operations.
- Implements a "fire-and-forget" mechanism.
- Atomic transactions using `BEGIN` and `COMMIT` blocks.
- Supports callback functions for query results.
- Comprehensive error management and graceful shutdown.

## Motivation
Traditional synchronous queries caused performance bottlenecks in our application. By creating a custom async processor within the `PostgresConnection` class, we improved efficiency without relying on third-party libraries.

## Workflow
1. **Queue Management**: Maintains queue for async queries and a separate bucket for pending(in-progress) queries.
2. **Async Queries**: Executes batches of queries without blocking.
3. **Result Handling**: Processes results via callbacks or error handlers.
4. **Shutdown Safety**: Processes all queries before application exit.

## Usage
1. Derive code in your programming language using enhanced `PostgresConnection` class pseudocode and integrate it into your project.
2. Use the `executeAsyncQuery` function to execute queries:
   ```php
   connection = new PostgresConnection();

   connection->executeAsyncQuery(
       "INSERT INTO users (name, email) VALUES ('John Doe', 'john@example.com')",
       "Error inserting user!",
       function ($result) {
           echo "User inserted successfully!";
       }
   );
   ```
3. Ensure all queries are processed using the `ProcessQueue` function at shutdown.

## Advantages
- Non-blocking database operations.
- Enhanced application responsiveness.
- Custom-tailored for PostgreSQL in applications based on any programming language.
