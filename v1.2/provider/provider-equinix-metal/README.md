# Crossplane Provider Equinix Metal v0.0.11 Conformance Results

The following steps can be undertaken to obtain the provided conformance results:

1. Create Kubernetes Cluster 

```
kind create cluster --name crossplane-conformance --image kindest/node:v1.19.11
```

2. Install Crossplane version 1.2.2

```
kubectl create namespace crossplane-system
helm repo add crossplane-stable https://charts.crossplane.io/stable
helm repo update
helm install crossplane --namespace crossplane-system crossplane-stable/crossplane --version 1.2.2
```

3. Deploy provider-equinix-metal version v0.0.11

```
kubectl crossplane install provider registry.upbound.io/equinix/provider-equinix-metal:v0.0.11
```


4. Create Equinix Metal ProviderConfig resource 

```
cat > /tmp/providerconfig.yaml <<EOF
---
apiVersion: v1
kind: Secret
metadata:
  name: example-provider-equinix-metal
  namespace: crossplane-system
type: Opaque
data:
  credentials: ${BASE64ENCODED_METAL_PROVIDER_CREDS}
---
apiVersion: metal.equinix.com/v1beta1
kind: ProviderConfig
metadata:
  name: equinix-metal-provider
spec:
  credentials:
    source: Secret
    secretRef:
        name: example-provider-equinix-metal
        namespace: crossplane-system
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
