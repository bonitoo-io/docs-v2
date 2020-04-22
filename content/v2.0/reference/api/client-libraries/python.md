---
title: Python client library
list_title: Python
description: >
  Use the Python client library to interact with InfluxDB.
menu:
  v2_0_ref:
    name: Python
    parent: Client libraries
v2.0/tags: [client libraries, python]
aliases:
  - /v2.0/reference/api/client-libraries/python-cl-guide/
weight: 201
---

Use the [InfluxDB Python client library](https://github.com/influxdata/influxdb-client-python) to integrate InfluxDB into Python scripts and applications.

This guide presumes some familiarity with Python and InfluxDB.
If just getting started, see [Getting started with InfluxDB](/v2.0/get-started/).

## Before you begin

1. Install the InfluxDB Python library:

    ```sh
    pip install influxdb-client
    ```

2. Ensure that InfluxDB is running.
If using InfluxDB Cloud, visit the URL of your InfluxDB Cloud UI.
For example: https://us-west-2-1.aws.cloud2.influxdata.com.
_For specific InfluxDB Cloud provider and region URLs, see [InfluxDB Cloud URLs](/v2.0/cloud/urls/)._
(If running InfluxDB locally, visit http://localhost:9999.)


## Write data to InfluxDB with Python

We are going to write some data in [line protocol](/v2.0/reference/syntax/line-protocol/) using the Python library.

In your Python program, import the InfluxDB client library and use it to write data to InfluxDB.

```python
import influxdb_client
from influxdb_client.client.write_api import SYNCHRONOUS
```

Next, we define a few variables with the name of your [bucket](/v2.0/organizations/buckets/), [organization](/v2.0/organizations/), and [token](/v2.0/security/tokens/).

```python
bucket = "<my-bucket>"
org = "<my-org>"
token = "<my-token>"
```

In order to write data, we need to create a few objects: a client, and a writer.
The InfluxDBClient object takes three named parameters: `url`, `org`, and `token`.
Here, we simply pass the three variables we have already defined.

```python
client = InfluxDBClient(
    url="https://us-west-2-1.aws.cloud2.influxdata.com",
    token=token,
    org=org
)
```

The `InfluxDBClient` object has a `write_api` method, used for configuration.
Instantiate a writer object using the `client` object and the `write_api` method.
Use the `write_api` method to configure the writer object.

```python
write_api = client.write_api(write_options=SYNCHRONOUS)
```

We need two more lines for our program to write data.
Create a [point](/v2.0/reference/glossary/#point) object and write it to InfluxDB using the `write` method of the API writer object.
The write method requires three parameters: `bucket`, `org`, and `record`.

```python
p = influxdb_client.Point("my_measurement").tag("location", "Prague").field("temperature", 25.3)
write_api.write(bucket=bucket, org=org, record=p)
```

For more information, see the [Python client README on GitHub](https://github.com/influxdata/influxdb-client-python).

### Complete example write script

```python
import influxdb_client
from influxdb_client.client.write_api import SYNCHRONOUS

bucket = "<my-bucket>"
org = "<my-org>"
token = "<my-token>"

client = influxdb_client.InfluxDBClient(
    url="https://us-west-2-1.aws.cloud2.influxdata.com",
    token=token,
    org=org
)

write_api = client.write_api(write_options=SYNCHRONOUS)

p = influxdb_client.Point("my_measurement").tag("location", "Prague").field("temperature", 25.3)
write_api.write(bucket=bucket, org=org, record=p)
```
