Проект v0.1: Local Optimization Service


solver-service/
├── app/
│   ├── main.py
│   ├── api/
│   ├── domain/
│   ├── solvers/
│   │   └── highs.py
│   │	└── scip.py
│   └── schemas/
├── tests/
├── Dockerfile
├── docker-compose.yml
└── pyproject.toml

Требования:

POST /v1/jobs
Принимает .lp и .mps
Запускает HiGHS
Возвращает job_id
GET /v1/jobs/{id} возвращает статус
После решения возвращает objective + значения переменных
Поддерживает time_limit
Поддерживает mip_gap
Имеет unit + integration tests
Всё запускается одной командой:
docker compose up


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
