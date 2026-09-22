# AmazIEEE - Back of envelope

Targets from our NFR: 1,000,000 users at a time, 400ms browsing, 100ms tracking, 300ms CRUD, 99.9% up.

## Assumptions

- 1,000,000 registered users, about 200,000 active on a busy day
- 500,000 product searches per day, 100,000 product reads per day
- 20,000 orders per day
- Sizes: order 8KB, notification 1KB, product in database 6kb, product 300KB in storage

## Requests per second

```
products browsing:     500,000 / 86400 = about 6 per sec avg, about 20 peak
products reads:  100,000 / 86400 = about 1 per sec avg, about 5 peak
order details:    20,000 / 86400 = about 0.2 per sec avg, about 1 peak
```

Peak edge is under 30 requests per sec. Two small API servers plus Redis handle this with room to spare. Latency is the hard part.

## Storage

```
orders : 200,000 x 6KB = 1.2 GB
notifications kept 90 days
products: 500,000 * 6kb = 3 gb
products in storage: 500,000 x 300KB = 150 GB
DB total well under 50 GB active. Fits on one Postgres node with replica.
Redis: allocated tasks and products, about 5 GB. One Redis node is enough.
```