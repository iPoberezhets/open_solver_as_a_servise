
SCIP
SCIP in docker with API.
SCIP_service can obtain .lp or .mps matrix and solve task.
diferent tasks: LP, IP, MILP


                    API Gateway
                         │
                         ▼
                 ┌──────────────┐
                 │ Optimization │
                 │    API       │
                 └──────┬───────┘
                        │
                     RabbitMQ
                        │
          ┌─────────────┼─────────────┐
          ▼             ▼             ▼
    ┌──────────┐  ┌──────────┐  ┌──────────┐
    │ HiGHS    │  │  SCIP    │  │   CBC    │
    │ Worker   │  │  Worker  │  │  Worker  │
    └──────────┘  └──────────┘  └──────────┘
          │             │             │
          └─────────────┼─────────────┘
                        ▼
                  PostgreSQL/S3
                        │
                        ▼
                  JSON solution