# Crossplane Provider AWS v0.19.0 Conformance Results

1. Install Crossplane v1.2.3 per [the Crossplane documentation](https://crossplane.io/docs/v1.2/getting-started/install-configure.html)
2. Install provider-aws v0.19.0 - `kubectl crossplane install provider crossplane/provider-aws:v0.19.0`
3. Create a ProviderConfig per https://github.com/crossplane/provider-aws/tree/v0.19.0/examples/providerconfig
4. Apply all example resources per https://github.com/crossplane/provider-aws/tree/v0.19.0/examples

Note that some example resources require human intervention (e.g. pointing them at real ARNs).