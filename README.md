## Tutorial for Kaniko

This tutorial will guide you through the process of building a Docker image using Kaniko. Kaniko is a tool to build container images from a Dockerfile, inside a container or Kubernetes cluster. It doesn't depend on a Docker daemon and executes each command within a Dockerfile completely in userspace. This enables building container images in environments that can't easily or securely run a Docker daemon, such as a standard Kubernetes cluster.

### Prerequisites

Before you begin, you'll need the following:
* A Kubernetes cluster
* kubectl installed and configured to use your cluster


### Step 1: Create a Namespace

Create a namespace for your Kaniko build:

```bash
kubectl create namespace kaniko
```

### Step 2: Create a Docker Registry Secret

Create a secret to authenticate with your Docker registry using kubectl:

```bash
kubectl create secret docker-registry myregistrykey \
  --docker-server=https://index.docker.io/v1/ \
  --docker-username=<your-docker-username> \
  --docker-password=<your-docker-password> \
  --docker-email=<your-email> \
  --namespace=kaniko
```

### Step 3: Create a Kaniko Pod

Create a Kaniko pod that builds a Docker image from a Dockerfile in this repository using kubectl:

```bash
kubectl apply -f kaniko-pod.yaml --namespace=kaniko
```

Change arguments in the `kaniko-pod.yaml` file to match your Docker registry and image name.

### Step 4: Check the Build Status

Check the status of the Kaniko build:

```bash
kubectl logs -f kaniko --namespace=kaniko
```

### Step 5: Verify the Image

Verify that the image was built successfully using docker pull:

```bash
docker pull <your-docker-username>/<your-image-name>
```

or using the Docker registry UI.