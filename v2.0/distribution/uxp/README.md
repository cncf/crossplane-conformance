# Upbound Crossplane (UXP) v2.0 Conformance Results

The following steps can be undertaken to obtain the provided conformance results:

1. Create Kubernetes Cluster

```
kind create cluster
```

2. Install UXP `2.0.2-up.4`

```
up install uxp 2.0.2-up.4
```

3. Set Crossplane Conformance Suite Version

```
xp_version=2.0
```

4. Run Crossplane Conformance Suite

```
sonobuoy run --plugin https://raw.githubusercontent.com/crossplane/conformance/release-${xp_version}/plugin-crossplane.yaml
```