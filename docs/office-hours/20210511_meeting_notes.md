# osquery office hours 2021-05-11

YouTube Link: https://youtu.be/ro5A8sL9zjI

## Announcements and Highlights since the last meeting

## Any Questions / Issues / PRs people want to discuss?

The intent of this section is to provide a clear time for community members to bring up _anything_.

Broad questions? Bugs? Deployment questions? Blocked PRs?

## aarch64 update

We are currently testing things on master post merge. This is enough that we now consider it "stable". 

We need to update the website to offer it as part of the downloads. 

What does stable mean?
* We have a loose commitment to supporting the platform
* tests run

### What about m1

Not yet clear what we need to port to the m1. 

seph thinks there's some porting work around the third party libraries

stefano thinks cmake might support this, but isn't sure how osquery's dependanies will work. 

## Moving install directory

It has been observed that on macOS, we install to `/usr/local/bin/osqueryd`. While homebrew commonly sets the permissions on `/usr/local/bin` to something user writable. So, how do feel about moving it? 

seph suggests `/opt/osquery` and maybe an `/usr/local/bin/osqueryi` symlink.

Also common on macOS `/Library/Application Support/`

version 5 is a breaking change, so it's a fine time to move the install dir. 

No one is opposed. (No one is really volunteering either)

### `.app` Bundle

Some discussion about what this entails

## Code Sign

Teddy is working a bit on this right now, and recommends we all focus on trying to get it done.

### macOS workflow

Notarization is pending

### uploads to S3?

Some discussion about how we 

## WAL causing latency

The issue describing the problem: https://github.com/osquery/osquery/issues/7079

We have a couple of cases where people are writing a high volume of events, run into some kind of issue around rocksdb. 

Some discussion about how we should interact with the WAL. Do we want it enabled or disabled? Do we want to flush every write, or ever N seconds? These are usually tradeoffs between performance and data consistency. In other database systems, this is usually exposed as a tunable.

There's an open question about how deleting works via the logging plugins. 

## FreeBSD and what constitutes a support platform

Someone was asking about bringing back freebsd support. 

Generally we're happy to accept patches, in keeping with the project, that enable building on other platforms.

But, without clear support from the community to address ongoing problems, it's hard to declare something stable. And we'd need CI.

## Look at old PRs 

_(If there's time, we've been trying to re-visit old PRs)_

[Reverse Sorted List of PRs](https://github.com/osquery/osquery/pulls?q=is%3Apr+is%3Aopen+sort%3Acreated-asc)
