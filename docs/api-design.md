| Endpoint                                 | Category      | Request-body                                    | Response            | status code    |
| ---------------------------------------- | ------------- | ----------------------------------------------- | ------------------- | -------------- |
| POST: /api/login                         | Auth          | email, password                                 | Token               | 200 OK         |
| POST: /api/register                      | Auth          | email, password, confirm-password, name         | Token               | 201 CREATED    |
| POST: /api/hire                          | Auth          | email, password, confirm-password, name         | Token               | 201 CREATED    |
| DELETE: /api/logout                      | Auth          | Token                                           | EMPTY               | 204 NO CONTENT |
| POST: /api/customer/orders               | Orders        | product-list, quantities, Token (Header)        | EMPTY               | 201 CREATED    |
| GET: /api/customer/orders                | Orders        | Token (Header)                                  | customer-order-list | 200 OK         |
| GET: /api/operator/orders                | Orders        | page, limit                                     | order-list          | 200 OK         |
| GET: /api/orders/{id}                    | Orders        | EMPTY                                           | order-details       | 200 OK         |
| GET: /api/orders/{id}/status             | Orders        | EMPTY                                           | robot-live-status   | 200 OK         |
| PATCH: /api/orders/{id}/status           | Orders        | status                                          | EMPTY               | 200 OK         |
| POST: /api/products                      | Products      | name, quantity, location: [shelf, zone], weight | EMPTY               | 201 CREATED    |
| GET: /api/products                       | Products      | page, limit                                     | product-list        | 200 OK         |
| GET: /api/products/{id}                  | Products      | EMPTY                                           | product-details     | 200 OK         |
| GET: /api/robots                         | Robots        | page, limit                                     | robot-list          | 200 OK         |
| POST: /api/robots                        | Robots        | name, battery-level                             | EMPTY               | 201 CREATED    |
| GET: /api/robots/{id}                    | Robots        | EMPTY                                           | robot-details       | 200 OK         |
| GET: /api/robots/{id}/state              | Robots        | EMPTY                                           | robot-live-state    | 200 OK         |
| PATCH: /api/robots/{id}/state            | Robots        | state of feet                                   | EMPTY               | 200 OK         |
| GET: /api/notifications                  | Notifications | page, limit                                     | notification-list   | 200 OK         |
| POST: /api/tasks                         | Tasks         | order-details                                   | Available-robot-id  | 201 CREATED    |
| POST: /api/tasks/assign                  | Tasks         | robot-id, order-id                              | EMPTY               | 200 OK         |
| POST: /api/notifications                 | Notifications | title, body, Token (Header)                     | EMPTY               | 201 CREATED    |
| PATCH: /api/notifications/{id}/mark-read | Notifications | EMPTY                                           | EMPTY               | 200 OK         |
| PATCH: /api/notifications/mark-all-read  | Notifications | EMPTY                                           | EMPTY               | 200 OK         |
