# Dapr Bindings (HTTP)

In this quickstart, you'll create a microservice to demonstrate Dapr's bindings API to work with external systems as inputs and outputs. The service listens to input binding events from a system CRON and then outputs the contents of local data to a PostreSql output binding. 

Visit [this](https://docs.dapr.io/developing-applications/building-blocks/bindings/) link for more information about Dapr and Bindings.

> **Note:** This example leverages only HTTP REST.  If you are looking for the example using the Dapr SDK [click here](../sdk).

This quickstart includes one service:
 
- Go service `app`

### Run and initialize PostgreSQL container

1. Open a new terminal, change directories to `../../db`, and run the container with [Docker Compose](https://docs.docker.com/compose/): 

<!-- STEP
name: Run and initialize PostgreSQL container
expected_return_code:
background: true
sleep: 5
timeout_seconds: 6
-->

```bash
cd ../../db
docker compose up
```

<!-- END_STEP -->

### Run Go service with Dapr

2. Open a new terminal, change directories to `./batch` in the quickstart directory and run: 

<!-- STEP
name: Install Go dependencies
-->

```bash
cd ./batch
go build .
```

<!-- END_STEP -->
3. Run the Go service app with Dapr: 

<!-- STEP
name: Run batch-http service
working_dir: ./batch
expected_stdout_lines:
  - '== APP == insert into orders (orderid, customer, price) values (1, ''John Smith'', 100.32)'
  - '== APP == insert into orders (orderid, customer, price) values (2, ''Jane Bond'', 15.40)'
  - '== APP == insert into orders (orderid, customer, price) values (3, ''Tony James'', 35.56)'
  - '== APP == Finished processing batch'
expected_stderr_lines:
output_match_mode: substring
sleep: 11
timeout_seconds: 30
-->
    
```bash
dapr run --app-id batch-http --app-port 6003 --dapr-http-port 3503 --dapr-grpc-port 60003 --components-path ../../../components -- go run .
```

<!-- END_STEP -->
