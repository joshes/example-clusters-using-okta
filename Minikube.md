# Minikube

## Requirements

- [minikube](https://github.com/kubernetes/minikube#installation)

For Minikube, we just need to start it and pass along the oidc-specific flags.

```sh
minikube start \
    --extra-config=apiserver.oidc-issuer-url=https://$OKTA_DOMAIN/oauth2/default \
    --extra-config=apiserver.oidc-client-id=$CLIENT_ID \
    --extra-config=apiserver.oidc-username-prefix=oidc: \
    --extra-config=apiserver.oidc-username-claim=sub \
    --extra-config=apiserver.oidc-groups-prefix=oidc: \
    --extra-config=apiserver.oidc-groups-claim=groups
```
