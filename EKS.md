### EKS

For EKS, I've copied the excellent bootstrap setup from https://github.com/hashicorp/learn-terraform-provision-eks-cluster and added a single config update to configure the authentication.

## Requirements

- [terraform](https://www.terraform.io/downloads.html)

Run the following from within the `eks` directory and be aware that this will create resources that cost money!

```sh
terraform init -upgrade

# This will take about 30m
terraform apply -var="client_id=$CLIENT_ID" -var="okta_domain=$OKTA_DOMAIN"

# Set the kubeconfig
export KUBECONFIG="$(pwd)/kubeconfig_$(terraform output -raw cluster_name)"
```

Make sure to tear down the resources when you're done!

```sh
terraform destroy 
```
