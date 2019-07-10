# Restoring the CLA

## Why is it required?

In short, to make sure that the Linux Foundation has full ownership and distribution rights of all committed code.

## Next steps

osquery previously used cla-assistant.io, which made sure the CLA was signed before anything could be merged. This integration is no longer present and has to be restored.

It is not possible to sign the CLA anymore from within GitHub, and the form present on the Facebook website is no longer applicable.

We are going to draft a new CLA based on the one from the Apache Foundation, but we'll of course have to get it approved by the Linux Foundation first.

Issue: https://github.com/osquery/foundation/issues/2

# Platform support

## Restoring previously supported platform

The prerelease version only supports Ubuntu 18.04+, Mojave and Windows 10. In the upcoming stable version we will try to restore support for at least the following operating systems:

 * CentOS 6, 7
 * Ubuntu 16.04, 16.10
 * Windows 7

# Next stable release

## Custom build toolchain

The "custom toolchain" is a build toolchain to build libc++ in a reproducible manner (exact-hash reproducible), and statically link the same libc++ library to all distributions of the osquery executable.

It is hard to ship binaries for Linux right now, as we basically have to compile them from scratch for each distribution we want to support. The custom build toolchain will solve this.

## Security updates

As the custom toolchain will make osquery depend on an old glibc version, in the future we should make sure that security updates on that library are applied.

# Build system changes

## Third-party dependencies

Dependencies need to be moved to a place that is accessible by the Foundation. The best approach is to keep using S3, as bandwidth and data at rest have a reasonable price.

The trouble with Git LFS is that it would require a lot of money (5 dollars for each 50 + 50 GiB of bandwidth and storage). Using git alone is not a really good alternative, as the repository would eventually become huge (and we likely also have a space limit).

# Packaging

## Signing keys

We are not yet able to solve this problem; we could ask the original Facebook maintainers to sign the binaries manually. This is not always doable though. The easiest approach is for them to build from source.

Keys are not in our possession yet, and things are still being negotiated with the Linux Foundation.

Windows and macOS signing keys are likely not transferable, so new ones will be generated. In order to obtain them, osquery needs to become an entity that can then register with the relevant developer program (Apple, Microsoft).

## Signing procedure

It is helpful to separate the packaging procedure in multiple isolated steps.

1. Build the project and generate a tarball
2. Take the tarball and package the contents
3. Take the package and sign it

## Distro-specific repositories

Packages are currently served via https from an S3 bucket on [pkg.osquery.io](https://pkg.osquery.io).

A plan should be made to migrate them to a new bucket that is accessible by the foundation.

## Chocolatey support

The osquery package on Chocolatey is still owned by Facebook, and we are not able to update it.

Supporting the ability to build MSI installers for osquery is a higher priority than supporting the Chocolatey package repository, but we would like to support both.

# Funding

## Open Collective

We may like to use https://opencollective.com/ to do transparent budgeting and accounting, and then use it to solicit funding from stakeholders. We can seek advisement from Linux Foundation about this.

Operational costs of the osquery project include:
* the osquery Slack channel
* establishing a legal entity that can register for developer programs at Microsoft, Apple
* code signing certs and their associated developer programs
* the CI infrastructure (Azure Pipelines)
* S3 hosting for cached binaries of pre-built dependencies
* website hosting (it's currently the no-cost GitHub Pages, but we'll move it back to Netlify in order to restore the ability to share deep links)

# Action items

As Linux releases are being affected by this issue, the highest priority right now is to prepare the custom toolchain and merge Teddy's rework for the third party dependencies.

* [Prepare a new CLA](https://github.com/osquery/foundation/issues/2) and have the Linux Foundation approve it
* Review [Teddy's third-party rework](https://github.com/osquery/osquery/pull/5616)
* Determine where the new binary libraries will be stored