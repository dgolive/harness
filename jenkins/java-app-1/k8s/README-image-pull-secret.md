# Artifactory image pull secret

The deployment uses private images from `trial5d3qns.jfrog.io`. Create a Kubernetes image pull secret so the cluster can authenticate to Artifactory.

## One-time setup

Create the secret in the **same namespace** as the deployment (`java-app-1`):

```bash
kubectl create secret docker-registry artifactory-registry-secret \
  --docker-server=trial5d3qns.jfrog.io \
  --docker-username=YOUR_ARTIFACTORY_USERNAME \
  --docker-password=YOUR_ARTIFACTORY_PASSWORD_OR_API_KEY \
  -n java-app-1
```

- **Username**: Your Artifactory username (or a dedicated service account).
- **Password**: Your Artifactory password or [Identity Token](https://jfrog.com/help/r/jfrog-platform-documentation/identity-tokens) (recommended for automation).

Ensure the user has at least **Read** on the `harness-vrepo` repository (or the repo where `java-app-1` is stored).

## Create namespace first (if needed)

```bash
kubectl create namespace java-app-1
```

Then create the secret as above.

## Verify

After creating the secret and deploying, pods should pull the image without 401 errors:

```bash
kubectl get pods -n java-app-1
kubectl describe pod -n java-app-1 -l app=java-app-1
```
