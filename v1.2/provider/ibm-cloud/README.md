# Provider-ibm-cloud

To reproduce the results follow these steps:

1. Install self-hosted Crossplane v1.2.1 on a Kind cluster with Kubernetes version v1.20.2
2. Install and configure the IBM Cloud Provider as described [here](https://github.com/crossplane-contrib/provider-ibm-cloud#install-ibm-cloud-provider)
3. Create a resources of each kind in the [examples directory](https://github.com/crossplane-contrib/provider-ibm-cloud/tree/master/examples). For resource instances, only the postgres example is required.
4. Follow the [conformance instructions](https://github.com/pdettori/crossplane-conformance/blob/main/instructions.md) to run the sonobuoy plugin and collect the results.