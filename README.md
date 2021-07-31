# example-clusters-using-okta

Examples of clusters using Okta for authentication.

## Requirements

- [Okta Account](https://developer.okta.com/signup/)
- [kubectl](https://kubernetes.io/docs/tasks/tools/#kubectl)
- [krew](https://krew.sigs.k8s.io/docs/user-guide/quickstart/)
- [kubelogin](https://github.com/int128/kubelogin)

Follow the links above to be certain of getting the right binaries for your system and creating an Okta account.

Follow along with my walkthrough over [here](https://medium.com/@bytecode/configure-cluster-access-via-okta-661d159efc5) to setup the Okta application.

Ensure you update the group name in `example-role.yaml` (It defaults to `Cluster Developers`). Just make sure it lines up with what you create in Okta.

Grab your `OKTA_DOMAIN` and `CLIENT_ID` and run the following, keeping the same shell throughout.

Note that `OKTA_DOMAIN` does not include `https://` or any trailing path(s). (e.g., `dev-12345678.okta.com`)

## Configure 

```sh
# Set these to your values
export CLIENT_ID="<SET ME>"
export OKTA_DOMAIN="<SET ME>"
export OIDC_USERNAME="oidc-user"
```

## Setup and configure a cluster

- [EKS](./EKS.md)
- [Minikube](./Minikube.md)

## Configure the role

```sh
# Create the in-cluster roles
kubectl apply -f example-role.yaml
```

## Configure the user

```sh
# Configure the oidc-login credentials
kubectl config set-credentials "$OIDC_USERNAME" \
    --exec-api-version=client.authentication.k8s.io/v1beta1 \
    --exec-command=kubectl \
    --exec-arg=oidc-login \
    --exec-arg=get-token \
    --exec-arg=--oidc-issuer-url=https://$OKTA_DOMAIN/oauth2/default \
    --exec-arg=--oidc-client-id=$CLIENT_ID \
    --exec-arg=--oidc-extra-scope=groups
```

## Test it out

You should now be able to list pods and namespaces 

```sh
kubectl get namespaces --user="$OIDC_USERNAME"
```
