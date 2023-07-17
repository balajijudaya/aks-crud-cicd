# Solution

## Crud APP

## Infra

cd infra
terraform init
terraform plan
terraform apply

resource_group_name=$(terraform output -raw resource_group_name)

az aks list \
  --resource-group $resource_group_name \
  --query "[].{\"K8s cluster name\":name}" \
  --output table

az ad sp create-for-rbac \
    --name "ghActionAzureRbac" \
    --scope /subscriptions/a940bee1-b520-434a-85a4-ab091a8ddf1e/resourceGroups/rg-tender-lioness \
    --role Contributor \
    --sdk-auth
echo "$(terraform output kube_config)" > ./azurek8s

export KUBECONFIG=./azurek8s

kubectl get nodes

