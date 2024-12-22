# Two-Tier Task Management Application

A simple task management application built with Flask and MySQL, containerized using Docker.

## Prerequisites

- Docker
- Docker Compose
- Git

## Step-by-Step Setup Instructions

1. Clone this repository:
```bash
git clone https://github.com/Harshil-kumar-4/twotier-app.git
cd twotier-app
```

2. Create a Docker network for communication between containers:
here "task-network" is network name that you going to use
```bash
docker network create task-network
```

3. Set up MySQL container:
here "taskdb" is the database name
```bash
# Pull MySQL image
docker pull mysql:8.0

# Run MySQL container
docker run -d \
  --name taskdb \
  --network task-network \
  -e MYSQL_ROOT_PASSWORD=root \
  -e MYSQL_DATABASE=taskdb \
  -p 3306:3306 \
  -v db-data:/var/lib/mysql \
  mysql:8.0
```

4. Build the Flask application image:
```bash
# Build the image
docker build -t task-app:latest .
```

5. Run the Flask application container:
here "task-app" is the backend image name and that going to continue with container
```bash
# Run the container
docker run -d \
  --name task-app \
  --network task-network \
  -e DB_HOST=taskdb \
  -e DB_USER=root \
  -e DB_PASSWORD=root \
  -e DB_NAME=taskdb \
  -p 5000:5000 \
  task-app:latest
```

6. Verify containers are running:
```bash
docker ps
```

7. Access the application at `http://localhost:5000`

## Using Docker Compose (Alternative Method)

If you prefer using Docker Compose go with the below command:
I recomend once go throug the compose file to understand--

```bash
docker-compose up --build
```

To stop and remove containers:
```bash
docker-compose down
```

## Container Management Commands

### Stop containers:
```bash
docker stop task-app taskdb
```

### Start existing containers:
```bash
docker start taskdb
docker start task-app
```

### Remove containers:
```bash
docker rm task-app taskdb
```

### Remove network:
```bash
docker network rm task-network
```

### View logs:
```bash
# View Flask app logs
docker logs task-app

# View MySQL logs
docker logs taskdb
```

## Features

- Create and view tasks
- Persistent storage using MySQL database
- Containerized application using Docker
- Container networking using Docker bridge network

## Security Scanning

To scan the Docker images for vulnerabilities using Docker Scout for this make shure the docker desktop is installed localy or in the server
Go to the "docker/scout-cli" in github check with the README file sure you will find the details in there
After instalation:
```bash
docker scout cves task-app:latest
```

## Architecture

- Frontend: Flask web application
- Backend: MySQL database
- Communication: Flask-SQLAlchemy ORM
- Network: Custom bridge network (task-network)

## Security Considerations

- Environment variables for sensitive data
- Secure network communication between containers
- Regular security scanning with Docker Scout
- Least privilege principle in container configuration

## Troubleshooting

1. If MySQL connection fails:
```bash
# Check if containers are running
docker ps

# Check if containers are on the same network
docker network inspect task-network

# Check MySQL logs
docker logs taskdb
```

2. If the application is not accessible:
```bash
# Check Flask app logs
docker logs task-app

# Verify port mapping
docker port task-app
```
## Contributing: 
Feel free to contribute to this project by submitting pull requests, add some additional features that are suitable or raising issues on GitHub.