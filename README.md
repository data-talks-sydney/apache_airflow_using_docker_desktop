[Running Airflow in Docker — Airflow Documentation (apache.org)](https://airflow.apache.org/docs/apache-airflow/stable/howto/docker-compose/index.html)


## What Is Apache Airflow?

[Apache Airflow](https://airflow.apache.org/) is a platform created by Airbnb that allows you to program, schedule and monitor queries from various data sources and perform simple treatments. It is written in Python and became open-source in 2015. It was later donated to the Apache Foundation. Airflow is an orchestrator of workflow flows that allows you to program, schedule and monitor queries from various data sources and perform simple treatments. Some examples of use cases include creating a schedule for periodic or non-periodic data migration from one table to another and importing data from various sources and unifying it into a centralized base.

## Airflow Architecture

Apache Airflow consists of the following main components that work in conjunction to author, schedule, execute, and monitor workflows.

1. **Scheduler:** Triggers scheduled tasks to run by sending them to the executer
2. **Executer:** Executes the workflows coming from the scheduler
3. **Webserver:** Hosts a simple user interface for monitoring and debugging workflows
4. **Dag Directory:** Stores your dag.py files that define workflows
5. **Metadata Database:** Stores metadata for each process to enable detailed logging


## Prerequisites:

#### Install Docker Desktop in your computer.

[Install Docker Desktop on Windows | Docker Documentation](https://docs.docker.com/desktop/install/windows-install/)

[Install Docker Desktop on Mac | Docker Documentation](https://docs.docker.com/desktop/install/mac-install/)

## Starting Airflow With Docker Compose

### **Step 1: Prepare Directories & Login Information**

We need to create our airflow directory and inside of that directory, create a folder called airflow.

MacOS
```zsh
$ mkdir Airflow && cd Airflow  
```

Windows
```powershell
> md Airflow && cd Airflow  
```

Now that we have our airflow folder, we must do the following: 

**a) Create three folders called dags, plugins and logs respectively**
	
Some directories in the container are mounted, which means that their contents are synchronized between your computer and the container.

- `./dags` - you can put your DAG files here.
- `./logs` - contains logs from task execution and scheduler.
- `./plugins` - you can put your [custom plugins](https://airflow.apache.org/docs/apache-airflow/stable/plugins.html) here.

MacOS
```zsh
$ mkdir -p ./dags ./logs ./plugins  
```

Windows
```powershell
> md dags 
> md logs 
> md plugins
```


**b) Prepare Login Information**

If you are in macOS or Linux you need to make sure that user and group permissions are the same between those folders from your host and the folders in your docker container.

MacOS
```zsh
echo -e “AIRFLOW_UID=$(id -u)\nAIRFLOW_GID=0” > .env
```

For other operating systems, you will get warning that `AIRFLOW_UID` is not set, but you can ignore it. You can also manually create the `.env` file in the same folder your `docker-compose.yaml` will be placed with this content to get rid of the warning:

> AIRFLOW_UID=50000

Windows (Windows Terminal)
```powershell
> echo “AIRFLOW_UID=50000\” > .env
```


### Step 2: Fetch docker-compose.yaml file

```powershell
> curl -LfO 'https://airflow.apache.org/docs/apache-airflow/2.6.3/docker-compose.yaml'

> tree /F 
.  
├── dags  
├── docker-compose.yaml  
├── logs  
└── plugins
```

### Step 3: Initialize Airflow

This command only needs to be run on your first initiation of the airflow service. It will setup the environment for you and create an admin user for you to login with.


```
docker compose up airflow-init
```

### Step 4: Run Docker Compose Up

Now that the Airflow environment has been initiated, we can launch it with the following command. The `-d` specifies detached.

```
docker compose up -d
```

### Step 5: Check Status of Containers

```
docker ps
```

### Step 6: Visit [_http://localhost:8080_](http://localhost:8080/) in your browser
