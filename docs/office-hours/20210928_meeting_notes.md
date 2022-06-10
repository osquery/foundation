# osquery office hours 2021-09-28

YouTube Link: https://youtu.be/b_FiAt6JpgU

## Announcements and Highlights since the last meeting

* 5.0.1 shipped! Seems anticlimactic

## Any Questions / Issues / PRs people want to discuss?

The intent of this section is to provide a clear time for community members to bring up _anything_.

Broad questions? Bugs? Deployment questions? Blocked PRs?

Some PRs from Alessandro:
* pid file PR -- https://github.com/osquery/osquery/pull/7304
  - Zach and seph will probably take a look
* BPF errors -- https://github.com/osquery/osquery/pull/7302
  - There are some comments in the PR about the occasional, expected, "malformed" errors

## Memory issues with ebpf (zacharycase & mattuebel)

https://github.com/osquery/osquery/issues/7310

Is there an expected memory increase when enabling bpf events? we've set our memory limit to 1G and we are seeing watchdog stop osqueryd every few minutes

Alessandro knows where these are, and is planning on taking a look next. We should expect this for 5.1

## How does osquery work anyhow

Someone is here with some questions about how osquery works. What is events expiry, etc.

A summary:
1. osquery is best thought of as an API translation layer. You issue queries, and osquery translates it to an underlying API. The "database" part is a bit of a misnomer. 
2. There's a bit exception / caveat to this -- Events.
3. When appropriate, osquery starts a "publisher" for a system evented API. This function will _publish_ to the internal rocksdb
4. The queries act as "subscriber" and reads data out the rocksdb
5. These queries generally track the "last seen" item, so they only show "new" entries, and not the whole saved events

Additionally, logging is buffered through rocksdb. 

Event expiration might be a bit weird, or at least unexpected. There are a lot of odd corners and surprises in this behavior. 

## Any issues post 5.0

* Per platform download links https://github.com/osquery/osquery-site/issues/227
* linux should put symlinks in `/usr`, not `/usr/local` https://github.com/osquery/osquery-packaging/issues/17
* MSI missing packs? https://github.com/osquery/osquery/issues/7327
  - Introduced in the 4.9 MSI changes
  - Probably easy
* Windows can't launch extensions -- https://github.com/osquery/osquery/issues/7324
* The upgrade can leave stale 4.9 artifacts on disk

Fleet and Kolide have both pushed bare binaries to their fleet. No customer complaints from them.

## Discussion about updating the contributor guidelines

As we see more and more PRs, we're thinking about them in the context of long term maintainability. 

When implementing new features:
* How can we scope this new feature?
* Is it exclusively using system API? Easier to review/accept,
  if it's a system artifact it's likely in scope. A blueprint
  issue is helpful but probably a quick discussion on Slack
  or during office hours is fine
  - Good candidate for a new feature
* Does it require new dependencies?
  - Which ones? Opening a blueprint issue is best.
  - How do we evaluate a new library?
  - What is the maintenance burden
  - Osquery is different
    + long term stability
    + security
* Does it require writing new custom libraries for undocumented
  formats?
  - Blueprint is probably mandatory
  - may not be a good candidate for core
  - What is the maintenance burden


When writing custom libraries, check this:

1. Assume osquery is targetted
2. Can it receive input from unauthenticated users?
3. Assume that input is badly formed, malicious
4. Malicious or corrupted formats
  - Large amount of structures
    - Memory, CPU usage
    - Stack exhaustion, in case recursion is used
  - Infinite loops
    - Structures can point to each other, creating infinite loops
  - Reading/writing out of bound
    - Invalid offsets, size fields

Likely TODOs:
* Documentation about how to write things for this
* More focus on blueprints to understand risk evaluation
  - Talk about it _before_ the contributor spends time on something we might reject
* Can we bring in sandboxing
  - Forking is an approach, though we tend to avoid forking
  - One direction would be long lived forks (akin to extensions), but this requires watchdog stuff
* What about binary parsing libraries
  - It's not clear we have a lot of use cases that would actually _use_ it. We don't do a lot of binary parsing today

## Look at old PRs 

- Just a reminder, this seems approved and only needs the codestyle check to pass https://github.com/osquery/osquery/pull/7234

- This is a trivial review, maybe we can approve today https://github.com/osquery/osquery/pull/7318

_(If there's time, we've been trying to re-visit old PRs)_

[Reverse Sorted List of PRs](https://github.com/osquery/osquery/pulls?q=is%3Apr+is%3Aopen+sort%3Acreated-asc)
