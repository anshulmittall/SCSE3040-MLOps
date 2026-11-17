# SCSE3040 Machine Learning Operations — Lab Work

Anshul — S24CSEU1052 — Bennett University, Session 2026-27

My solved lab practicals for SCSE3040. Every practical uses the same
600-row food-delivery dataset (`data/delivery_times.csv`) and predicts how
many minutes an order will take.

| Folder | Practical | What is inside |
|---|---|---|
| `P01-workbench` | Your MLOps workbench | pinned requirements, run record |
| `P02-first-model` | First model | notebook |
| `P03-model-choice` | Choosing a model | notebook |
| `P04-package` | Packaging the code | `delivery/` package, `train.py`, `predict.py` |
| `P05-config-tests` | Config file and tests | `config.yaml`, pytest suite |
| `P06-mlflow` | Experiment tracking | MLflow tracking database |
| `P07-serving` | FastAPI service | `app.py` |
| `P08-docker` | Docker | `Dockerfile`, `.dockerignore` |
| `P09-kubernetes` | Kubernetes manifests | Deployment, Services |
| `P10-logging` | Structured logging | JSON log file |
| `P11-ci` | Continuous integration | GitHub Actions workflow |
| `P12-pipeline` | Pipelines (DAG) | daily report |
| `P13-monitoring` | Monitoring and drift | incident note |

Each folder has the notebook `PXX_S24CSEU1052.ipynb` with every cell run,
plus the files it wrote in `work/`.

## The deployed service

`deploy/` is the FastAPI service from P07. It is deployed on FastAPI Cloud:

**https://delivery-api-s24cseu1052.fastapicloud.dev/docs**

`pyproject.toml` and `.python-version` (Python 3.12) are what FastAPI Cloud
builds from; `Dockerfile` (from P08) runs the same service in a container and
reads the port from `$PORT`.

Deploy again after a change:

```bash
cd deploy
fastapi deploy
```

Run it locally with Docker:

```bash
cd deploy
docker build -t delivery-api .
docker run --rm -p 8000:8000 delivery-api
# then open http://127.0.0.1:8000/docs
```

Endpoints: `GET /health`, `GET /model-info`, `POST /predict`.
A request outside the allowed ranges (for example `traffic_level` 9 or a
distance above 25 km) is refused with status 422.
