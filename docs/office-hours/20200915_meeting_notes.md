# osquery office hours 2020-09-15

YouTube Link: https://youtu.be/GPYZEOvk1Hg

## Announcements and Highlights since the last meeting

### 4.5.0 is in beta

* windows32!
* arm64!

Packages are on in Slack, see #core channel.

Right now, we do not currently expect to distribute windows32 or arm64
packages.

### Apple Silicon

Apple has approved us for Apple Silicon hardware. It is enroute to
seph's desk, and when it arrives, he'll figure out access.

Dependancies that use raw assembly, need to have support for apple
silicon. Boost is probably one of these, likely zlib as well.

This was graciously covered by Kolide. Thanks!

### AWS Credit for arm64

As part of the arm64 support seph has requested AWS credit to run
graviton2 machines.

## Any Questions / Issues / PRs people want to discuss?

The intent of this section is to provide a clear time for community
members to bring up _anything_.

Broad questions? Bugs? Deployment questions? Blocked PRs?

## Support for newer macOS builds

We discussed wanting to support newer macOS released. While we target
10.11, we run CI on a 10.14 machine. We should add runners for 10.15,
and 11.0 when available.

Folks aren't sure if current versions of XCode have build
errors. MikeM has tested building and running osquery tests with Xcode
12.0 beta and 11.7, without issues.

## Do we have a focus for 4.6.0?

One suggestion is that we try to focus on fixing/closing some of the
issues.

Apple Silicon is going to take some attention.

ToB has a lot of requests for improvements around the event
backends. We are likely to see PRs to this end. Alessandro has been
talking about this for awile.

## Endpoint Security in macOS

Discussion about what we need to do for Endpoint Security API support.

Apple has granted us use of the Endpoint Security entitlement. So we
just need to start using it.

Alessandro says he ran into some issues getting profiles to work when
signing with CMake.

## Hacktober fest is coming

Is there anything we want to do? Tag issues? Stick our name somewhere?

## Look at old PRs

_(If there's time, we've been trying to re-visit old PRs)_

[Reverse Sorted List of PRs](https://github.com/osquery/osquery/pulls?q=is%3Apr+is%3Aopen+sort%3Acreated-asc)
