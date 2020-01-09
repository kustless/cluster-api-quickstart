# cluster-api-quickstart

[Quick Start - The Cluster API Book](https://cluster-api.sigs.k8s.io/user/quick-start.html)

```sh
# Download the latest binary of clusterawsadm from the AWS provider releases and make sure to place it in your path.
curl -sSLO https://github.com/kubernetes-sigs/cluster-api-provider-aws/releases/download/v0.4.7/clusterawsadm-darwin-amd64

# Setup [AWS Provider Prerequisites](https://github.com/kubernetes-sigs/cluster-api-provider-aws/blob/master/docs/prerequisites.md)
clusterawsadm alpha bootstrap create-stack

# Create the base64 encoded credentials using clusterawsadm.
# This command uses your environment variables and encodes
# them in a value to be stored in a Kubernetes Secret.
export AWS_B64ENCODED_CREDENTIALS=$(clusterawsadm alpha bootstrap encode-aws-credentials)

# Create the components.
curl -L https://github.com/kubernetes-sigs/cluster-api-provider-aws/releases/download/v0.4.7/infrastructure-components.yaml \
  | envsubst \
  | kubectl create -f -

# Create a target cluster
kustomize build apply -f -
```

Deploy a CNI solution

```sh
kubectl get secret/capi-quickstart-kubeconfig -o json \
  | jq -r .data.value \
  | base64 --decode \
  > ./capi-quickstart.kubeconfig

kubectl --kubeconfig=./capi-quickstart.kubeconfig \
  apply -f https://docs.projectcalico.org/v3.8/manifests/calico.yaml
```
