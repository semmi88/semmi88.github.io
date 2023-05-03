# Serverless and Traditional compute clashing

### Background

Our engineering team at Aircall favors using managed services and serverless technology in order to quickly deliver new functionalities. For example, our teams heavily rely on AWS Lambda, Function-as-a-Service, for implementing business logic.

You can learn more about our tech stack in another blog entry. Lambda functions are an excellent choice for most scenarios, as they scale out-of-the-box based on traffic, are charged per usage, and can also be orchestrated into complex workflows using AWS Step Functions. However, there are some situations when their usage is not so straightforward. Let's dive deeper into one such case: connecting to a relational database.

The well-established way of interacting with a relational database is by using a connection pool. One reason for this is that opening and closing database connections is a costly operation, using up database memory and compute resources, and doing it at a high rate (on every request) can greatly reduce performance. So a better way is to pool together connections and reuse them on subsequent requests, this way reducing overhead. A connection pool can also serve for limiting the maximum number of concurrent database connections, when there are no free connections to take from the pool, clients will wait for one to be available (or eventually timeout).

However, to maintain a connection pool, we need a long-lived, always-running server, which is not the case for AWS Lambdas. Lambdas are executed in short-lived execution environments (micro virtual machines), which are created on demand and destroyed automatically after the function execution (or in some cases several function executions). This means we cannot simply just use traditional connection pooling, and we are left with opening and closing connections on every invocation.

The question then arises, are there better ways of relational database connection when using serverless on-demand compute? Let's look at two such options, which promise to improve performance:

    serverless-mysql - an open-source npm library, a wrapper around the MySQL client, with connection management specifically thought up for serverless applications, allowing concurrent executions to share connections

    AWS RDS Proxy - a paid AWS service, which provides a fully managed database proxy, allows for pooling and sharing of database connections, and can be enabled for most applications with no code changes

### Test set up

To compare and evaluate these different options, we ran a test scenario simulating a typical use case in our system.

First, we deployed a relational database (AWS Aurora backed by MySQL), with one instance (db.t4g.medium and a max_connection parameter set to 170). Then we created a table for storing Call Transcription–related information, with an auto-incremented primary key transcription_id, and several other columns. For these tests, we inserted around 1 million records into the database.

In the second step, we deployed three different Lambdas, each with its own way of handling database connections:

1. Simple approach - open/close DB connection on every invocation inside the lambda handler - using mysql2 npm library
```
import mysql2 from "mysql2/promise";

export const lambdaHandler = async (event:any): Promise<any> => {

    const connection = await mysql2.createConnection({db_host_url, user, pass})
    const [rows, fields] = await connection.query(randomQuery());
    //...
    await connection.end();
};
```

2. Using serverless-mysql library with default configurations - DB connection created once outside the lambda handler and reused, while query() and end() calls are executed on every invocation inside the lambda handler

```
import mysql from "serverless-mysql";

const connection = await mysql.createConnection({db_host_url, user, pass})

export const lambdaHandler = async (event: any): Promise<any> => {
  
    const results: Array<any> = await connection.query(randomQuery());
    //...
    await connection.end();
};
```
3. Using RDS Proxy with default configurations - same as the first approach, uses mysql2 library, but instead of connecting directly to the DB host url, we are connecting to the Proxy endpoint
```
import mysql2 from "mysql2/promise";

export const lambdaHandler = async (event:any): Promise<any> => {

    const connection = await mysql2.createConnection({proxy_url, user, pass})
    const [rows, fields] = await connection.query(randomQuery());
    //...
    await connection.end();
};
```

A typical query pattern is that we want to retrieve details for one specific call transcription based on transcription_id, so that is going to be our test logic, in the Lambda handlers we request one record based on a random transcription_id. All of these Lambdas are fronted by an API Gateway, allowing the use of standard HTTP benchmarking tools for testing (autocannon).

Because the Lambdas will scale seamlessly the bottleneck during our testing will be the max allowed database connections (170), and the difference will be made in how efficiently these connections are managed. So to stress test this, performance was evaluated while executing a fixed amount of work (10.000 database queries based on primary key), using a different number of concurrent connections.

We also set a latency goal, wanting to make sure that the 99th percentile of response times is below 500 milliseconds (a threshold taken from current production metrics). This means, that out of our 10.000 requests, we want to exclude the worst/slowest 1 percent (~100 calls) and measure the maximum response time for the remaining 99% of the calls.

To mitigate the effect of Lambda cold starts on the test results, before every test run a 30 seconds warm-up is executed. To mitigate the effects of random network delays, all tests were executed twice and the best results were taken from these runs.

autocannon --connections <CONNECTIONS> --duration 30 <API_ENDPOINT> // warm-up 30 sec 
autocannon --connections <CONNECTIONS> --amount 10000 --latency <API_ENDPOINT> // test run #1
autocannon --connections <CONNECTIONS> --amount 10000 --latency <API_ENDPOINT> // test run #2

### Test results and evaluation

```
    Fixed amount of work: 10.000 requests

    Latency goal: p99 < 500ms (excluding 1%, ~ 100 slowest requests)

    Increasing number of concurrent connections (max 170 connections supported)
```

In the table above you can find the final results. Each row represents the test runs for a different way of handling database connections. The columns represent the runs with an increasing number of concurrent connections. Each cell contains the resulting latencies (p99, p99.9), the average throughput (request/second), and also the total execution time for the 10.000 requests.

Looking at the results is a bit underwhelming, as based on this test setup, there is no significant difference between the three options. All of them perform well, within the latency objective of up to 100 connections or so, and start degrading above that in a similar fashion, a clear sign that the database is under connection pressure. (As a side note, when using the serverless-mysql library it is important to call the end() function after finishing the queries, or else the connection management is not handled correctly and we can easily get Too many connections errors.)

So we can say that at this scale there is no difference between these options. We could try testing at higher throughput and use a bigger database instance, or add more reader instances. It would also be interesting to fine-tune connection management configurations instead of using the default ones. However, this test setup is representative of our typical use case in production (peak load of 100 requests/second, 50 maximum concurrent executions), and so in our case, it looks like there is no need for fancy connection management. We can just stick with the simplest possible thing that can work: opening and closing database connections on every request.
