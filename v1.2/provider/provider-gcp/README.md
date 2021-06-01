# Crossplane Provider GCP v0.17.1 Conformance Results

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

3. Deploy provider-gcp version v0.17.1

```
kubectl crossplane install provider crossplane/provider-gcp:v0.17.1
```


4. Create GCP ProviderConfig resource 

```
cat > /tmp/providerconfig.yaml <<EOF
---
apiVersion: v1
kind: Secret
metadata:
  namespace: crossplane-system
  name: provider-gcp
type: Opaque
data:
  credentials: ${BASE64ENCODED_GCP_PROVIDER_CREDS}
---
apiVersion: gcp.crossplane.io/v1beta1
kind: ProviderConfig
metadata:
  name: default
spec:
  projectID: crossplane-playground
  credentials:
    source: Secret
    secretRef:
      namespace: crossplane-system
      name: provider-gcp
      key: credentials
EOF

kubectl apply -f "/tmp/providerconfig.yaml"
```

5. Create composition definitions

```
kubectl apply -f composition
```

5. Create composite resource

```
kubectl apply -f resources/conformancetest.yaml
```

6. Run Crossplane Conformance Suite

```
xp_version=1.2
sonobuoy run --plugin "https://raw.githubusercontent.com/crossplane/conformance/release-${xp_version}/plugin-provider.yaml"
```

8. Check status/logs

```
sonobuoy status
sonobuoy logs
```

7. Cleanup 

```
kubectl delete -f resources
```
