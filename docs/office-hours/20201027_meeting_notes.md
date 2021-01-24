# osquery office hours 2020-10-27

YouTube Link: https://youtu.be/adbBE_8q9SE

## Announcements and Highlights since the last meeting

## Any Questions / Issues / PRs people want to discuss?

The intent of this section is to provide a clear time for community
members to bring up _anything_.

Broad questions? Bugs? Deployment questions? Blocked PRs?

## User/developer guide to logging levels and error details

User/developer guide to logging levels and error details: in osquery
there's some confusion among logging levels (INFO, WARNING, ERROR,
--verbose). Levels where one would not expect errors (INFO) contain
them etc. What one should expect from INFO log messages? And WARNING?
And ERROR? And..

Discussion about the info level:
* startup messages about what subsystems are enabled (event providers and whatnot) might make sense in INFO
* Should it include errors? (we appear mixed)
* Errors are not always clear, they often need more information
* Info should probably be understandable from a user perspectie
* Normal runtime is Info, and it should be be quiet

For example, [`process_open_sockets`](https://github.com/osquery/osquery/pull/6546) returns data that is a `uint64`, but sqlite only supports `int64`. So there is _always_ a cast error here. It's not clear what the right approach should be.

## Fixing the Thrift build error

The [initial BPF implementation
PR](https://github.com/osquery/osquery/pull/6571) introduced C++17
support which broke the Thrift library on some systems. We have a
[PR6732](https://github.com/osquery/osquery/pull/6732) ready that
should address the problem.

How did this get through CI? cmake treated these are different args,
and sometimes cmake may shuffle args around. We think we just got
lucky in the initial CI.

Some discussion that we should make it easy for developers to mirror
the cmake environment that CI uses.

## Container support for BPF (ready for review)

[PR6721](https://github.com/osquery/osquery/pull/6721) introduces
container support for fork/vfork/clone events when using the BPF event
publisher previously introduced in
[PR6571](https://github.com/osquery/osquery/pull/6571).

This is partly a call to action to test BPF.

## Endpoint Security

Sharvil and seph have been playing a bit with the provisioning
profile.

It seems to work for Shavil, but seph is having TCC issues.

What are the next steps here:

1. We need a prodution provisioning profile
2. How does this work in CI? Does the CI runner have a fixed or variable Device ID?
   * Initial testing from stefano looks like a consistent hardware info
3. Does CI run with SIP enabled?
   * as of 2019-May, yes https://developercommunity.visualstudio.com/idea/558702/allow-disabling-sip-on-microsoft-hosted-macos-agen.html
   * Stefano confirms yes. 

For CI to test the endpoint security stuff, we need to sign the
builds. But we don't want to sign PRs with a distribution cert. We
could sign with a developer cert, but that only works if the CI has a
consistent device id. Some research needed.

## Docker Emulated aarch64

Docker can emulate aarch64, will this work for CI? (This is full
emulation)

No. It's way to slow for CI

## The chocolatey package validation

This validation process has changed, so 4.5.1 isn't up there. Nick is
going to take a look at this.

Relatedly, https://github.com/osquery/osquery/issues/6691 asks us to
adjust the build process to push metadata into a new windows download
thing.

## Look at old PRs 

_(If there's time, we've been trying to re-visit old PRs)_

[Reverse Sorted List of PRs](https://github.com/osquery/osquery/pulls?q=is%3Apr+is%3Aopen+sort%3Acreated-asc)
