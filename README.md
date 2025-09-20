# 🛒 NF ACADEMY Microservices Demo on Kubernetes

This project is a deployment of the **Google Cloud Microservices Demo**
("Hipster Shop") running on Kubernetes using **Helm** and **Helmfile**.\
It simulates a real-world online e-commerce store with multiple
microservices written in different languages.

------------------------------------------------------------------------

## 🚀 Application Overview

The demo application includes multiple services that together form a
functioning web store:

-   **frontend** → Exposes the web UI to users.
-   **productcatalogservice** → Provides product data such as names,
    descriptions, and prices.
-   **cartservice** → Stores user carts in Redis.
-   **recommendationservice** → Provides product recommendations based
    on the product catalog.
-   **currencyservice** → Converts currencies in real time.
-   **checkoutservice** → Handles checkout by talking to payment,
    shipping, email, and cart services.
-   **paymentservice** → Dummy credit card payment service.
-   **shippingservice** → Quotes shipping costs and handles orders.
-   **emailservice** → Sends order confirmation emails.
-   **adservice** → Provides product ads.
-   **redis-cart** → Redis instance backing the `cartservice`.

------------------------------------------------------------------------

## 🛠️ Tools & Technologies Used

-   **Kubernetes** → Container orchestration platform.
-   **Helm** → Kubernetes package manager to define, install, and
    upgrade services.
-   **Helmfile** → Manage multiple Helm releases with one configuration.
-   **Minikube** → Local Kubernetes cluster for development and testing.
-   **Docker** → Containers for all services.
-   **Google Microservices Demo images** → Prebuilt container images for
    each microservice.
-   **Redis** → In-memory store used by `cartservice`.

------------------------------------------------------------------------

## ⚙️ Deployment

### 1. Prerequisites

-   [Minikube](https://minikube.sigs.k8s.io/docs/start/) installed and
    running.
-   [kubectl](https://kubernetes.io/docs/tasks/tools/) CLI installed.
-   [Helm](https://helm.sh/docs/intro/install/) installed.
-   [Helmfile](https://github.com/helmfile/helmfile) installed.

### 2. Clone Repository

``` bash
git clone git@github.com:YOUR_USERNAME/microservice-practice.git
cd microservice-practice
```

### 3. Start Minikube

``` bash
minikube start --cpus=4 --memory=8192
```

### 4. Deploy Services with Helmfile

``` bash
helmfile sync
```

Or install manually per service:

``` bash
helm upgrade --install frontend ./frontend -f values/frontend-values.yaml
```

### 5. Access the Frontend

If using Minikube:

``` bash
minikube service frontend
```

Or if `LoadBalancer` is pending, expose as NodePort:

``` bash
kubectl expose deployment frontend --type=NodePort --port=80 --target-port=8080
minikube service frontend
```

------------------------------------------------------------------------

## 📦 Repository Structure

    .
    ├── helmfile.yaml                # Defines all Helm releases
    ├── values/                      # Helm values for each microservice
    │   ├── frontend-values.yaml
    │   ├── productcatalog-values.yaml
    │   ├── cartservice-values.yaml
    │   ├── ...
    └── README.md                    # This file

------------------------------------------------------------------------

## 🧩 Notes

-   Some services require environment variables (`*_SERVICE_ADDR`,
    `REDIS_ADDR`, etc.) defined in their respective `values.yaml`.

-   If you see `CrashLoopBackOff`, check pod logs:

    ``` bash
    kubectl logs <pod-name>
    ```

-   If services can't talk to each other, verify `targetPort` and
    `env vars` match the container port.

------------------------------------------------------------------------

## 📖 References

-   [Google Cloud Microservices
    Demo](https://github.com/GoogleCloudPlatform/microservices-demo)
-   [Helmfile Docs](https://helmfile.readthedocs.io/)
-   [Kubernetes Documentation](https://kubernetes.io/docs/home/)

------------------------------------------------------------------------
