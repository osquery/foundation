# osquery office hours 2022-03-15

YouTube Link: https://www.youtube.com/watch?v=81fKVyfBrls

## Announcements and Highlights since the last meeting

* 5.3 release for some dependency upgrades (https://github.com/osquery/osquery/issues/7494)
* 5.2.2 now in Chocolatey

## Any Questions / Issues / PRs people want to discuss?

The intent of this section is to provide a clear time for community members to bring up _anything_.

Broad questions? Bugs? Deployment questions? Blocked PRs?

## Status of FreeBSD support?

It seems that we no longer support FreeBSD in any form -- the osquery port also expired two years ago. But we mention it on readthedocs, osquery.io downloads page, and the schema page. That could lead to confusion..should we remove those references?

* We don't think anyone has build it at least since buck, and maybe not even then. Adding support is likely a significant amount of work to the build system.
* Full support for it would require CI
* General sentiment in favor of removing it from the docs/website/etc
* Less sure about removing the `ifdef` blocks from the code. They might be stale, but they seem easy to keep. 

## BPF and other event publishers

Under the hood, Linux does most things by file descriptors. But, FDs are basically pointers, which aren't really
human readable. So to make it human readable, there's a lot of work in tracking the FD resolution. And some of the
resolution is single threaded, which is _very_ hard if something is running 50 containers. 

1. ToB is seeing people deploy osquery on container servers. 
2. Tracing the file descriptors used is very heavy
3. What are other options?

* What about only outputing the filename and not the full path?
  - Better performance, but potentially less useful info
  - But full path may be meaningless given mount namespaces
* What about joining to a fd_resolve table?
  - This would create race conditions, since FDs are transcient. 

There is general support for whatever Alessandro wants to do.

Some side conversation about about how to support experiments. The drivers here primarily to support testing things on deployments that only want official binaries. So how to we support things? There's some support for simply shipping tables. There's also support for an `--enable-expirment=XXXX` flag for things that need gating. Only concerns for the latter are that we not break flag compatibility when we drop an expirment. 

## Look at old PRs 

_(If there's time, we've been trying to re-visit old PRs)_

[Reverse Sorted List of PRs](https://github.com/osquery/osquery/pulls?q=is%3Apr+is%3Aopen+sort%3Acreated-asc)
