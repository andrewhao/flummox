## isomorphic-influx

A library for writing stats to InfluxDB 2.0. Metrics can fire from either a Node server backend or from a browser.

## Key concepts

Be sure to review the [key concepts from the Influx documentation](https://v2.docs.influxdata.com/v2.0/reference/key-concepts/data-elements/) (fields, tags). Be aware that this library only supports InfluxDB 2.0 metrics, and not InfluxDB 1.0.

## Usage

```js
import { writeMetric } from "isomorphic-influx";

writeMetric(
  metricName, // Name of the metric you are logging
  (fieldSet = {}), // Fields to log, as an object
  (tagSet = {}), // Tag metadata to log, as an object
  (INFLUXDB_ORG_ID = process.env.INFLUXDB_ORG_ID), // InfluxDB Org ID
  (INFLUXDB_BUCKET_ID = process.env.INFLUXDB_BUCKET_ID), // InfluxDB Bucket ID
  (INFLUXDB_SERVER_URL = process.env.INFLUXDB_SERVER_URL), // InfluxDB server base URL
  (INFLUXDB_TOKEN = process.env.INFLUXDB_TOKEN) // InfluxDB account token
);
```

Under the hood, `writeMetric` is simply a `fetch` request, so you can chain a `then` handler for the success case, or a `catch` handler for request errors.

```js
writeMetric(
  "temperature_updates",
  {
    temperature: 72.0,
    humidity: 50.0,
    isFanOn: true
  },
  {
    device_id: "raspberry-pi"
  }
)
  .then(() => {
    // Handle success
    console.log("Success!");
  })
  .catch(err => {
    // Handle errors
    console.error("Error updating metric", err);
  });
```
