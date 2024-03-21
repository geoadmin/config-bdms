# # Kubernetes Deployment

The application is deployed to a Kubernetes cluster using Helm. Deployments, services, configmaps, secrets, and ingress routes are defined in the Helm chart. The application is deployed to a dedicated namespace to isolate it from other applications running on the same cluster.

## Connect to Kubernetes Cluster

## Prerequisites

1. Create secret and access key for your [AWS account](https://us-east-1.console.aws.amazon.com/iamv2/home?region=us-east-1#/security_credentials?section=IAM_credentials)
2. Download, install and configure [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
3. Download, install and configure [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl-windows/#install-nonstandard-package-tools)
4. Download, install and configure [helm](https://helm.sh/docs/intro/install/)

## Validate Helm Chart

Validate with

```bash
helm lint .\swissgeol-boreholes
```

or pretend to install the chart to the cluster and if there is some issue it will show the error.

```bash
helm install --dry-run swissgeol-boreholes .\swissgeol-boreholes
```

## Install Helm Chart

Use the default values to deploy the application (not recommended)

```bash
helm install swissgeol-boreholes .\swissgeol-boreholes \
  --namespace 'swissgeol-boreholes' \
  --create-namespace
```

or use some basic custom values (recommended) to deploy the application

```bash
helm install swissgeol-boreholes .\swissgeol-boreholes \
  --namespace 'swissgeol-boreholes' \
  --create-namespace \
  --set app.domain="dev-boreholes.swissgeol.ch" \
  --set app.version="edge"
```

For a full list of values, you can check the `values.yaml` file. Use the `--set` flag to override the default values. 

```bash
helm install swissgeol-boreholes .\swissgeol-boreholes \
  --namespace 'swissgeol-boreholes' \
  --create-namespace \
  --set app.domain="dev-boreholes.swissgeol.ch" \
  --set app.version="edge" \
  --set database.host="dbhost" \
  --set database.name="dbname" \
  --set database.username="dbusername" \
  --set database.password="dbpassword" \
  --set s3.endpoint="bucketEndpoint" \
  --set s3.bucket="bucketName" \
  --set s3.accessKey="awsAccessKey" \
  --set s3.secretKey="awsSecretKey" \
  --set auth.authority="https://auth.example.com" \
  --set auth.audience="authClientId"
```

## Upgrade Helm Chart

After making changes to the application configuration, update the version number in the `Chart.yaml` file and run the following command to update the application.

```bash
helm upgrade swissgeol-boreholes .\swissgeol-boreholes \
  --namespace 'swissgeol-boreholes' \
  --reuse-values
```

## Uninstall Application

To delete the application, you can use the following command

```bash
helm uninstall swissgeol-boreholes \
  --namespace 'swissgeol-boreholes'
```
