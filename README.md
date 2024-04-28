[![](https://onlineboutique.dev/static/icons/Hipster_NavLogo.svg)](https://onlineboutique.dev/)

# **Introduction**
**Online Boutique** is a cloud-first microservices demo application deployed by Rajinikanth Vadla. Online Boutique consists of an 11-tier microservices application. The application is a web-based e-commerce app where users can browse items, add them to the cart, and purchase them.

Google uses this application to demonstrate the use of technologies like Kubernetes, GKE, Istio, Stackdriver, and gRPC. This application works on any Kubernetes cluster, like Google
Kubernetes Engine (GKE). It’s **easy to deploy with little to no configuration**.

**Credits:** [Link to GitHub repository](https://github.com/GoogleCloudPlatform/microservices-demo)

## Screenshots

| Home Page                                                                                                         | Checkout Screen                                                                                                    |
| ----------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------ |
| [![Screenshot of store homepage](docs/img/online-boutique-frontend-1.png)](docs/img/online-boutique-frontend-1.png) | [![Screenshot of checkout screen](docs/img/online-boutique-frontend-2.png)](docs/img/online-boutique-frontend-2.png) |

## Architecture

**Online Boutique** is composed of 11 microservices written in different
languages that talk to each other over gRPC.

[![Architecture of
microservices](docs/img/architecture-diagram.png)](docs/img/architecture-diagram.png)

| Service                                              | Language      | Description                                                                                                                       |
| ---------------------------------------------------- | ------------- | --------------------------------------------------------------------------------------------------------------------------------- |
| [frontend](https://github.com/GoogleCloudPlatform/microservices-demo/tree/main/src/frontend)                           | Go            | Exposes an HTTP server to serve the website. Does not require signup/login and generates session IDs for all users automatically. |
| [cartservice](https://github.com/GoogleCloudPlatform/microservices-demo/tree/main/src/cartservice)                     | C#            | Stores the items in the user's shopping cart in Redis and retrieves it.                                                           |
| [productcatalogservice](https://github.com/GoogleCloudPlatform/microservices-demo/tree/main/src/productcatalogservice) | Go            | Provides the list of products from a JSON file and ability to search products and get individual products.                        |
| [currencyservice](https://github.com/GoogleCloudPlatform/microservices-demo/tree/main/src/currencyservice)             | Node.js       | Converts one money amount to another currency. Uses real values fetched from European Central Bank. It's the highest QPS service. |
| [paymentservice](https://github.com/GoogleCloudPlatform/microservices-demo/tree/main/src/paymentservice)               | Node.js       | Charges the given credit card info (mock) with the given amount and returns a transaction ID.                                     |
| [shippingservice](https://github.com/GoogleCloudPlatform/microservices-demo/tree/main/src/shippingservice)             | Go            | Gives shipping cost estimates based on the shopping cart. Ships items to the given address (mock)                                 |
| [emailservice](https://github.com/GoogleCloudPlatform/microservices-demo/tree/main/src/emailservice)                   | Python        | Sends users an order confirmation email (mock).                                                                                   |
| [checkoutservice](https://github.com/GoogleCloudPlatform/microservices-demo/tree/main/src/checkoutservice)             | Go            | Retrieves user cart, prepares order and orchestrates the payment, shipping and the email notification.                            |
| [recommendationservice](https://github.com/GoogleCloudPlatform/microservices-demo/tree/main/src/recommendationservice) | Python        | Recommends other products based on what's given in the cart.                                                                      |
| [adservice](https://github.com/GoogleCloudPlatform/microservices-demo/tree/main/src/adservice)                         | Java          | Provides text ads based on given context words.                                                                                   |
| [loadgenerator](https://github.com/GoogleCloudPlatform/microservices-demo/tree/main/src/loadgenerator)                 | Python/Locust | Continuously sends requests imitating realistic user shopping flows to the frontend.                                              |

# Deploy the Online Boutique Microservices in a Kubernetes cluster
## Prerequisites
- Create Account on any Managed Kubernetes Cluster Provider (e.g. EKS, DigitalOcean, AWS, Azure, GKE, etc..). We will be using EKS as our managed kubernetes cluster provider.
- Install [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/) in your local machine
- Install [helm](https://helm.sh/docs/intro/install/) in your local machine
- Install [helmfile](https://helmfile.readthedocs.io/en/latest/#installation) in your local machine

## Deploying the micro-services using [Helm Charts](charts/) and [Helmfile](helmfile.yaml) into EKS Kubernetes Cluster

Steps:
1. Steps 1-5 will be same as above.
2. Create [Helm Charts](charts/) and [Helmfile](helmfile.yaml).
3. Deploy the Helm Charts using Helmfile:

    helmfile sync -f online-shop-microservices/helmfile.yaml

    ![Deployment of micro-services using helmfile](docs/img/deploy-using-helmfile.PNG)

The log from helmfile sync command will be huge but it is good to go through it to understand what all services got deployed

The service can now be accessed using the any of the Node IP inside the cluster with the frontend service port number
![Check the webpage](docs/img/webpage-manifest.PNG)
| Home Page                                                                                                         | Checkout Screen                                                                                                    |
| ----------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------ |
| ![Screenshot of store homepage](docs/img/webpage-manifest.PNG) | ![Screenshot of checkout screen](docs/img/checkputpage-manifest.PNG) |
