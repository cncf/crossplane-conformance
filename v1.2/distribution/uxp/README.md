# Upbound Universal Crossplane (UXP) 1.2 Conformance Results

The following steps can be undertaken to obtain the provided conformance results:

1. Create Kubernetes Cluster

```
kind create cluster
```

2. Install UXP v1.2.1-up.3

```
up install uxp 1.2.1-up.3
```

3. Set Crossplane Conformance Suite Version

```
xp_version=1.2
```

4. Run Crossplane Conformance Suite

```
sonobuoy run --plugin https://raw.githubusercontent.com/crossplane/conformance/release-${xp_version}/plugin-crossplane.yaml
```
