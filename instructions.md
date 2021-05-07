# How to submit conformance results

## The tests

The standard set of conformance tests is currently those defined by the
[crossplane/conformance] Sonobuoy plugin.

## Running

The standard tool for running these tests is [Sonobuoy]. Download a [binary
release] of the CLI, or build it yourself by running:

```console
go get -u -v github.com/vmware-tanzu/sonobuoy
```

Deploy a Sonobuoy pod to your cluster with:

```console
# The version of Crossplane your wish to validate conformance against.
xp_version=1.2

# To validate the conformance of a Crossplane distribution.
sonobuoy run --plugin https://raw.githubusercontent.com/crossplane/conformance/${xp_version}/plugin-crossplane.yaml

# To validate the conformance of a Crossplane provider.
sonobuoy run --plugin https://raw.githubusercontent.com/crossplane/conformance/${xp_version}/plugin-provider.yaml

# To view the status of the conformance tests.
sonobuoy status

# To view Sonobuoy logs.
sonobuoy logs
```

Once `sonobuoy status` shows the run as `completed`, copy the output directory
from the main Sonobuoy pod to a local directory:

```console
outfile=$(sonobuoy retrieve)

# Extract the contents into `./results`
mkdir ./results; tar xzf ${outfile} -C ./results
```

> The two files required for submission are located in the tarball as
> `plugins/crossplane-conformance/results/global/{report.log,report.xml}`.

To clean up Kubernetes objects created by Sonobuoy, run:

```console
sonobuoy delete
```

## Uploading

Prepare a PR in this repository. Here are [directions] to prepare a pull request
from a fork. In the descriptions below, `X.Y` refers to the Crossplane major and
minor version, and `$dir` is a short subdirectory name to hold the results for
your product. Examples would be `distribution/coolplane` or `provider/pizza`.

Description: `Conformance results for vX.Y/$dir`

### Contents of the PR

For simplicity you can submit the tarball or extract the relevant information
from the tarball to compose your submission.

If submitting test results for multiple versions, submit a PR for each product,
ie. one PR for vX.Y results and a second PR for vX.Z

```
vX.Y/$dir/README.md: A script or human-readable description of how to reproduce
your results.
vX.Y/$dir/sonobuoy.tar.gz: Raw output from sonobuoy. (optional)
vX.Y/$dir/report.log: Test log output (from Sonobuoy).
vX.Y/$dir/report.xml: Machine-readable test log (from Sonobuoy).
vX.Y/$dir/PRODUCT.yaml: See below.
```

#### PRODUCT.yaml

This file describes your product. It is YAML formatted with the following
root-level fields. Please fill in as appropriate.

| Field               | Description                                                                                                                                                                                       |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `vendor`            | Name of the legal entity that is certifying. This entity must have a signed participation form on file with the CNCF.                                                                             |
| `name`              | Name of the product being certified.                                                                                                                                                              |
| `version`           | The version of the product being certified (not the version of Crossplane it runs).                                                                                                               |
| `website_url`       | URL to the product information website.                                                                                                                                                           |
| `repo_url`          | If your product is open source, this field is necessary to point to the primary GitHub repo containing the source. It's OK if this is a mirror. OPTIONAL.                                         |
| `documentation_url` | URL to the product documentation.                                                                                                                                                                 |
| `product_logo_url`  | URL to the product's logo, (must be in SVG, AI or EPS format -- not a PNG -- and include the product name). OPTIONAL. If not supplied, we'll use your company logo. Please see [logo guidelines]. |
| `type`              | Is your product a distribution of Crossplane or a Crossplane provider? (Specify one of `distribution` or `provider`.)                                                                             |
| `description`       | One sentence description of your offering.                                                                                                                                                        |

Examples below are for a fictional Crossplane distribution called _Coolplane_
produced by a company named _Compu-Hyper-Global-Meganet_.

```yaml
vendor: Compu-Hyper-Global-Meganet
name: Coolplane
version: v1.7.4
website_url: https://compuhyperglobalmega.net/coolplane
repo_url: https://github.com/chgmnet/cooplane
documentation_url: https://compuhyperglobalmega.net/coolplane/docs
product_logo_url: https://compuhyperglobalmega.net/assets/coolplane.svg
type: distribution
description: "Coolplane is the coolest distribution of Crossplane."
```

## Amendment for Private Review

If you need a private review for an unreleased product, please email a zip file
containing what you would otherwise submit as a pull request to
conformance@cncf.io. We'll review and confirm that you are ready to be Certified
Crossplane as soon as you open the pull request. We can then often arrange to
accept your pull request soon after you make it, at which point you become
Certified Crossplane.

## Review

A reviewer will shortly comment on and/or accept your pull request, following
this [process]. If you don't see a response within 3 business days, please
contact conformance@cncf.io.

## Example Script

Combining the steps provided here, the process looks like this:

```
$ xp_version=v1.2
$ prod_name=coolplane
$ type=distribution
$ results_dir=${xp_version}/${type}/${prod_name}

$ go get -u -v github.com/vmware-tanzu/sonobuoy

$ sonobuoy run --wait
$ outfile=$(sonobuoy retrieve)
$ mkdir ./results; tar xzf $outfile -C ./results

$ mkdir -p ./${results_dir}
$ cp ./results/plugins/e2e/results/global/* ./${results_dir}

$ cat << EOF > ./${results_dir}/PRODUCT.yaml
vendor: Compu-Hyper-Global-Meganet
name: Coolplane
version: v1.7.4
website_url: https://compuhyperglobalmega.net/coolplane
repo_url: https://github.com/chgmnet/cooplane
documentation_url: https://compuhyperglobalmega.net/coolplane/docs
product_logo_url: https://compuhyperglobalmega.net/assets/coolplane.svg
type: distribution
description: "Coolplane is the coolest distribution of Crossplane."
EOF
```

## Issues

If you have problems certifying that you feel are an issue with the conformance
program itself (and not just your own implementation), you can file an issue in
this repository.

[crossplane/conformance]: https://github.com/crossplane/conformance
[sonobuoy]: https://github.com/vmware-tanzu/sonobuoy
[binary release]: https://github.com/vmware-tanzu/sonobuoy/releases
[directions]: https://docs.github.com/en/github/collaborating-with-issues-and-pull-requests/creating-a-pull-request-from-a-fork
[logo guidelines]: https://github.com/cncf/landscape#logos
[process]: reviewing.md
