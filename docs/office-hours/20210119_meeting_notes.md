# osquery office hours 2021-01-19

YouTube Link: https://youtu.be/qKTKktZ_tqo

## Announcements and Highlights since the last meeting

### Github actions merged!

### osquery@scale

[Zach](https://twitter.com/thezachw) is going to talk at [osquery@scale](https://www.osqueryatscale.com/) tomorrow, about eBPF and the future of osquery on Linux! If you don't have a ticket already, you are missing out!

Teddy is speaking!

## Signing Pipeline

We have github actions, but we have to protect the signing secrets.

The current mockup here involves a separate repo https://github.com/osquery/osquery-codesign 

This is currently a private repo to reduce the likelihood of outside interference. seph is interested in having this become public.

Workflow (proposed, not all implemented)
1. When started, gets tag from the input
2. finds artifacts by tag
3. downloads to working directory
4. (signging should happen here)
5. puts signed packages on the release tag

Package signing has some wrinkles -- We need to unpack, get the binary, sign, repack, sign. This might change later, but it's the manual process we do today, and we want to stick with what works.

Discussion about how we want to have a single packaging workflow, so that we can create non-codesigned packages and codesigne dones using the same workflow

### What about signing in CI?

We still need to create a developer profile for CI, so we can bake signing in. Also need to resolve whether the 'device ID' for a CI runner is static, because developer-signing requires a device ID to be pre-provisioned with Apple and added to the profile.

## Move/copy the Docker images?

GitHub actions uses a docker container coming out of ToB. This installs packages similar to what's in the wiki, and in CI. 

seph and Stefano are chatting about github docker registry. 

## Adding CentOS to GitHub Actions

We should be able to do this, but it'll need to happen on the docker images. 

## State of 4.6

Kolide is running it in their beta fleet. No issues, so we're removing the "pre-release" tag, and declaring it stable. 

## Please test the new librpm

Teddy asks if people can test https://github.com/osquery/osquery/pull/6850 with more rpm based linuxes

Zach can look

## Update macOS min version

https://github.com/osquery/osquery/pull/6896

We can set the SDK and minimum version seperately. 

We'd like to upgrade the minimum version, so that we can use the `available` macros.


Our minimum is currently 10.11. But we need to update this because we need weak linking. It's worth noting that even testing on 10.12 is very hard, since we cannot easily

Using macOS 10.12 as the deployment target is needed for weak linking, for availability macros, for building the table that exposes the Unified Logging log, etc.


## Look at old PRs 

_(If there's time, we've been trying to re-visit old PRs)_

[Reverse Sorted List of PRs](https://github.com/osquery/osquery/pulls?q=is%3Apr+is%3Aopen+sort%3Acreated-asc)
