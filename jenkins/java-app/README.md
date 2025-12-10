# Java Kubernetes Demo Application

A simple Spring Boot Java application designed to run in Kubernetes with a Jenkins CI/CD pipeline.

## Application Features

- Spring Boot 3.2.0 REST API
- Health check endpoint (`/api/health`)
- Hello endpoint (`/api/hello`)
- Actuator endpoints for monitoring
- Runs on port 8080

## Project Structure

```
java-app/
├── src/
│   └── main/
│       ├── java/com/example/demo/
│       │   ├── DemoApplication.java
│       │   └── HelloController.java
│       └── resources/
│           └── application.properties
├── k8s/
│   ├── deployment.yaml
│   └── service.yaml
├── Dockerfile
├── jenkinsfile
├── pom.xml
└── README.md
```

## Building Locally

### Prerequisites
- Java 17+
- Maven 3.6+

### Build and Run

```bash
# Build the application
mvn clean package

# Run the application
java -jar target/demo-1.0.0.jar

# Or run with Maven
mvn spring-boot:run
```

### Test the Application

```bash
# Health check
curl http://localhost:8080/api/health

# Hello endpoint
curl http://localhost:8080/api/hello
```

## Docker Build

```bash
# Build Docker image
docker build -t java-app:latest .

# Run container
docker run -p 8080:8080 java-app:latest
```

## Kubernetes Deployment

### Prerequisites
- kubectl configured to access your Kubernetes cluster
- Docker image available in cluster (or use local registry)

### Deploy

```bash
# Apply Kubernetes manifests
kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml

# Check deployment status
kubectl get deployments
kubectl get pods
kubectl get svc

# Port forward to test locally
kubectl port-forward svc/java-app-service 8080:80
```

### Access the Application

```bash
# Via port-forward
curl http://localhost:8080/api/hello

# Or via service (if accessible)
curl http://java-app-service/api/hello
```

## Jenkins Pipeline

The Jenkinsfile includes the following stages:

1. **Checkout** - Retrieves source code
2. **Build** - Compiles Java application with Maven
3. **Test** - Runs unit tests
4. **Build Docker Image** - Creates container image
5. **Deploy to Kubernetes** - Applies Kubernetes manifests
6. **Verify Deployment** - Checks deployment status

### Jenkins Setup

1. Ensure Jenkins has:
   - Maven plugin installed
   - Docker plugin installed
   - Kubernetes CLI (kubectl) available
   - Kubernetes credentials configured (if needed)

2. Configure pipeline:
   - Create a new Pipeline job in Jenkins
   - Point to the Jenkinsfile in this repository
   - Adjust environment variables as needed:
     - `DOCKER_REGISTRY` - Your Docker registry URL
     - `KUBERNETES_NAMESPACE` - Target Kubernetes namespace

3. Run the pipeline:
   - The pipeline will build, test, containerize, and deploy the application

### Customization

- **Docker Registry**: Uncomment and configure Docker registry push in the Jenkinsfile
- **Namespace**: Change `KUBERNETES_NAMESPACE` environment variable
- **Image Tagging**: Modify `IMAGE_NAME` to use tags like `latest`, `v${BUILD_NUMBER}`, etc.

## Endpoints

- `GET /api/hello` - Returns a greeting message with timestamp
- `GET /api/health` - Health check endpoint
- `GET /actuator/health` - Spring Boot Actuator health endpoint

## Resources

- Spring Boot: https://spring.io/projects/spring-boot
- Kubernetes: https://kubernetes.io/
- Jenkins: https://www.jenkins.io/
