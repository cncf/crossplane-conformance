# Crossplane Provider Helm v0.7.2 Conformance Results

The following steps can be undertaken to obtain the provided conformance results:

1. Create Kubernetes Cluster 

```
kind create cluster --name crossplane-conformance --image kindest/node:v1.19.11
```

2. Install Crossplane version 1.2.1

```
kubectl create namespace crossplane-system

helm repo add crossplane-stable https://charts.crossplane.io/stable
helm repo update

helm install crossplane --namespace crossplane-system crossplane-stable/crossplane --version 1.2.1
```

3. Deploy provider-helm v0.7.2

```
kubectl crossplane install provider crossplane/provider-helm:v0.7.2
```


4. Create ProviderConfig resource 

```
cat > /tmp/providerconfig.yaml <<EOF
---
apiVersion: helm.crossplane.io/v1beta1
kind: ProviderConfig
metadata:
  name: default
spec:
  credentials:
    source: InjectedIdentity
EOF

kubectl apply -f "/tmp/providerconfig.yaml"
```

Make sure provider-helm has enough permissions to make deployments into the same cluster

```
SA=$(kubectl -n crossplane-system get sa -o name | grep provider-helm | sed -e 's|serviceaccount\/|crossplane-system:|g')
kubectl create clusterrolebinding provider-helm-admin-binding --clusterrole cluster-admin --serviceaccount="${SA}"
```


5. Create a helm release resource

```
kubectl apply -f resources/release.yaml
```

6. Run Crossplane Conformance Suite

```
xp_version=1.2
sonobuoy run --plugin "https://raw.githubusercontent.com/crossplane/conformance/release-${xp_version}/plugin-provider.yaml"
```

7. Check status/logs

```
sonobuoy status
sonobuoy logs
```
