# How to submit conformance results

@negz is fleshing out the test instructions and expected results.  This page will contain the
details for how to run the conformance tests and verifications then submit them for review.

## General Areas of Verification

### Distributions

For the certification of conformant Distributions of Crossplane, the verification tests will confirm
the following:

* `kubectl crossplane install` successfully installs standard Provider and Configuration packages
  * All installed packages reconcile to a healthy and running state
* All XRDs and CRDs defined in the packages are installed into the control plane and reconcile to the `established` and `offered` state
* `CompositeResources` defined in the packages are created and
  * A standard XR can be created which results in the instantiation of the `Composition`'s base resources
  * XR configuration values are successfully patched down to composed Managed Resources

### Providers

For the certification of conformant Providers, the verification tests will confirm the following:

* Provider can be installed into upstream Crossplane using `kubectl crossplane install provider`
* CRDs from the Provider are available in the cluster's API server after the Provider is installed
* Managed Resources from the Provider expose standard status conditions
* Lifecycle management of a managed resource emits events

## Uploading the Results

The observations and results from executing the conformance tests should be submitted in the form of
a [pull request](https://github.com/cncf/crossplane-conformance/pulls) to this repository.

## Issues

If you have problems certifying that you feel are an issue with the conformance program itself (and
not just your own implementation), you can file an issue in [this
repository](https://github.com/cncf/crossplane-conformance/issues).

Questions and comments can also be discussed with the Crossplane community in the [Slack
workspace](http://slack.crossplane.io/), preferably the #dev channel.
