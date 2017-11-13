# APIC-Sizing-API-project

This is the api which will power the APIC Sizing app intended to streamline and speed up sizing of basic and moderately complex requests. 

## DB Installation

Before running the project, the database must be imported to Mongo. The database contains benchmarks and reference data required to complete the sizing. The database dump is located under ./apicSizingDb. The DB can be imported with the command

```sh
mongorestore -d db_name path/
```

## Model Architecture

### Custom DP Sizing

This is required when the seller needs a custom sizing and NOT a T-shirt sizing.

Custom DP Sizing is executed via the sizings/dpSizing function. The input is described in the table below:

| Parameter | Description |
| ------ | ------ |
| numTransactions | number of API transactions in a given frequency |
| frequency | Frequency of the number of transactions parameter. The options are only "second", "day", "week", "month". Anything else will give an error. |
| requestSize | This is measured in KB. Currently options are only 5, 8, 16, 32, 64, 128, 256, 512, or 1024. |
| gwType | Array of type of gateway. Options are only "physical" and/or "virtual" |
| latencyMS (optional) | the maximum API latency accepted in MS. This has not yet been included in query. DO NOT TEST YET. |
| concurrency | number of concurrent API calls. NOT YET FUNCTIONAL. DO NOT TEST YET. |
| scenario | There are 4 type of scenario defined by the sizing tech team. The options are only ""Simple Scenario", "Common Scenario", "Complex Scenario (XML Input)", "Complex Scenario (JSON Input)"|

Sample input below :

```sh
{
  "numTransactions": 4000,
  "frequency": "second",
  "requestSize": 16,
  "gwType": [
    "virtual","Physical"
  ],
  "latencyMS": 0,
  "concurrency": 0,
  "scenario": "Common Scenario"
}
```

Produces this output, where amount is the number of nodes and the gwConfig is the hardware required per node:

```sh
[
  {
    "amount": 10,
    "gwConfig": {
      "NIC": "10Gbit",
      "RAM": "16GB",
      "concurrency": 10,
      "cores": 8,
      "firmware": "IDG.7.5.1.0 build rel-7-5-1-branch.277405",
      "name": "VE-8core",
      "type": "Virtual"
    }
  },
  {
    "amount": 4,
    "gwConfig": {
      "NIC": "10Gbit",
      "concurrency": 50,
      "firmware": "IDG.7.5.1.0 build rel-7-5-1-branch.277405",
      "name": "IDG-50Clients",
      "type": "Appliance"
    }
  }
]
```
