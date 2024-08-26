# Deploying MLFlow using Docker

MLFlow is an open-source MLOps platform which provides Experiment Tracking, Model Store, and Deployment modules, among other things. They've recently added support for LLM models as well.

If you've used a service like Weights and Biases or a library/tool like Tensorboard, the experiment tracking UI will feeel familiar. By adding a few lines of code to your jupyter notebooks and python scripts, you can track the progress of machine learning model training over time. Typically variables like loss is tracked over time but you can monitor any variable in your python code. At the end of a model training session, you can also use mlflow's model storage feature to upload trained model variants to the mlflow tracking server.

## Getting started

To get started with using mlflow in your own local environment, the easiest way I've found is to use the mlflow-docker-compose git repository. Specifically, I've used the fork maintained by CallMeMSL:  [https://github.com/sachua/mlflow-docker-compose](https://github.com/CallMeMSL/mlflow-docker-compose)).

The reason I use this repository is that it is the most recently updated one when filtering for "recently updated" in the main repository (https://github.com/sachua/mlflow-docker-compose/forks?include=active&page=1&period=2y&sort_by=last_updated).

The main difference is that he uses postgres as the backend database instead of mysql.

## Requirements

You will need to have docker engine installed and make sure that the docker compose or docker-compose command works.


## Running

Simply follow the instructions in the readme to use the docker-compose.yml file to build and run the relevant docker containers.

```
docker-compose up -d --build
```

Once up, you can access the mlflow tracking server UI on port 5000 and the mino server on port 9000.

THe default credentials for the mlflow server should be mlflow as the username and an empty password.

Make sure to change this password in the .env file before running the docker-compose command.

## Troubleshooting

try using "docker compose" if "docker-compose" doesn't work.
