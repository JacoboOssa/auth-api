# Authentication API

This part of the exercise is responsible for the users authentication.
- `POST /login` - takes a JSON and returns an access token

The JSON structure is:
```json
{
    "username": "admin",
    "password": "admin"
}
```

## Configuration

The service scans environment for variables:
- `AUTH_API_PORT` - the port the service takes.
- `USERS_API_ADDRESS` - base URL of [Users API](/users-api).
- `JWT_SECRET` - secret value for JWT token processing. Must be the same amongst all components.

## Initial data
Following users are hardcoded for you:

|  Username | Password  |
|-----------|-----------|
| admin     | admin     |
| johnd     | foo       |
| janed     | ddd       |

## Building

```
- export GO111MODULE=on
- go mod init github.com/bortizf/microservice-app-example/tree/master/auth-api
- go mod tidy
- go build
```

## Running
```
 JWT_SECRET=PRFT AUTH_API_PORT=8000 USERS_API_ADDRESS=http://127.0.0.1:8083 ./auth-api
```

## Usage
In case you need to test this API, you can use it as follows:
```
 curl -X POST  http://127.0.0.1:8000/login -d '{"username": "admin","password": "admin"}'
```

## Dependencies
Here you can find the software required to run this microservice, as well as the version we have tested. 
|  Dependency | Version  |
|-------------|----------|
| Go          | 1.18.2   |

## CI/CD Pipeline

This project includes an Azure DevOps pipeline defined in the `azure-pipelines.yml` file. The pipeline automates the following steps:

1. **Build and Push Stage**:
   - Logs in to the Azure Container Registry (ACR).
   - Builds the Docker image for the `auth-api` service.
   - Pushes the image to the ACR with two tags: the build ID and `latest`.

2. **Deployment**:
   - Deploys the newly built Docker image to an Azure Container App.

### Pipeline Variables

- `vmImageName`: Specifies the VM image used for the build agent (e.g., `ubuntu-latest`).
- `dockerRegistryServiceConnection`: The Azure DevOps service connection for ACR.
- `acrLoginServer`: The login server for the ACR.
- `imageRepository`: The name of the Docker image repository.
- `dockerfilePath`: Path to the Dockerfile.
- `buildContextPath`: Path to the build context.
- `tag`: The tag for the Docker image (e.g., build ID).
- `containerAppName`: The name of the Azure Container App.
- `resourceGroupName`: The resource group containing the Container App.
- `azureResourceManagerConnection`: The Azure DevOps service connection for Azure Resource Manager.

### Running the Pipeline

To trigger the pipeline, push changes to the `main` branch (or the default branch specified in the `trigger` section of the pipeline). The pipeline will automatically build, push, and deploy the `auth-api` service.

## Adding the Pipeline to Azure DevOps

To set up the CI/CD pipeline for this project in Azure DevOps, follow these steps:

1. **Create a New Pipeline**:
   - Log in to your Azure DevOps account.
   - Navigate to your project and go to the "Pipelines" section.
   - Click on "New Pipeline."

2. **Select the Repository**:
   - Choose the repository where your project is hosted (e.g., GitHub, Azure Repos).

3. **Configure the Pipeline**:
   - Select the option to use an existing YAML file.
   - Point to the `azure-pipelines.yml` file in your repository.

4. **Save and Run**:
   - Save the pipeline configuration.
   - Run the pipeline to ensure it works as expected.

5. **Monitor the Pipeline**:
   - Check the pipeline's progress and logs in the Azure DevOps interface.

By following these steps, you can integrate the CI/CD pipeline into Azure DevOps and automate the build, push, and deployment processes for the `auth-api` service.