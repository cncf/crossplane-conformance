# How to review conformance results

## Technical Requirements

1. Verify that the list of files matches the [expected list].

2. Note the `vX.Y` subdirectory that the PR is in, this is the version of
   Crossplane for which conformance is being claimed, referenced as the
   "Conformance Version" from hereon.

3. Verify that the Conformance Version is the current or previous two versions
   of Crossplane.

4. Look at `results.log`. Verify that the `major.minor` component of the
   `Conformance test version` exactly matches the Conformance Version. The patch
   version does not matter.

5. Verify that the last line of `results.log` says `PASS`.

6. Verify that no files outside of the submitted subdirectory are being
   modified. For example, if a submission is adding files to
   `v1.2/distribution/coyote/`, ensure that it doesn't change `README.md` or
   `v1.2/distribution/roadrunner/`.

## Policy Requirements

1. Confirm that the vendor is currently a [member] of CNCF. If they are not,
   request that they reach out to memberships@cncf.io to become a member in
   order to complete their certification. (Companies can alternatively pay a
   certification fee equal to the cost of membership if, for whatever reason,
   they don't wish to become a CNCF member.) Alternatively, non-profit
   organizations (including community distributions like Debian) can certify at
   no cost.

Review the `PRODUCT.yaml` file, and:

2. Verify that there is a Participation Form on file for the `vendor`, and that
   the vendor is in good standing in the program.

3. Verify the product `name` and `website_url` are listed in the "Qualifying
   Offerings" section of the vendor's Participation Form, i.e. that the `name`
   and `website_url` are listed.

4. Review the `name`. If it contains the word "Kubernetes" in a non-descriptive
   fashion, verify that the `name` is listed in "Participant Kubernetes
   Combinations" section of the vendor's Participation Form.

If the submission doesn't meet all policy requirements, reply with a message
indicating "Signed participation form needed", "Files missing from PR",
"Membership in CNCF or confirmation of non-profit status needed", etc.

## Tasks to Complete After Review

1. Add a comment saying "You are now Certified Crossplane" and merge the PR.

[expected list]: instructions.md#contents-of-the-pr
[member]: https://www.cncf.io/about/members/
