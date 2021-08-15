# osquery office hours 2019-08-04

YouTube Link: https://youtu.be/XSrtt7RcJIs

## Announcements and Highlights since the last meeting

## Any Questions / Issues / PRs people want to discuss?

The intent of this section is to provide a clear time for community
members to bring up _anything_.

Broad questions? Bugs? Deployment questions? Blocked PRs?

## Endpoint Security

This PR is pretty close to being mergable. So that might land next
week. (yay!) It sounds like they've got next steps.

We have the endpoint security entitlement.

There are lots of moving parts in this, but we haven't strung them all
together.

We will probably manually sign some test releases to get started. But
we'll want to get a CI build for something official.

## header reorg

Teddy is almost ready. This will be 700 files. It will require any
open PRs to be manually rebased. Targeting this week. He may ask for
help tomorrow.

seph can make a copyright change PR pretty fast after that's merged.

## New Apple Changes

Samuel Keeley has a DTK. ToB might as well. It sounds like there's
some work ahead there.

## Check in on ongoing big changes

### BPF Pull request

Alessandro has made the pull request. They have a customer they're
doing some debugging with.

There's a testing failure around running as a normal user vs needing
root. There might be a test hardness failure. GTEST has some undefined
behaviors around whether an error is handled.

### Windows 32bit

We really wany some CI for this. Otherwise we think we'll end up
accidentally using `size_t` when we mean `uint64_t`

Do we know what the CI flags would be? Might just be handing a
different toolchain to cmake.

### ARM64 support

(https://github.com/osquery/osquery/pull/6336)

Josh@vmware says they have some resources to help get move this
forward.

* We want to **not** disrupt x86_64 builds.
* We can do this piecemeal -- it's okay to land incompletely/broken aarch64 support
* The config files in this PR may need to be regenerated with the
  toolchain (we can probably merge without)
* We'll need CI. Looks like Azure supports this now.
* We should create issues for the followup items.
* We target glib 2.12+ we're unsure what the oldest glib with arm64 support.

## Heisenbug around event subscribers being added twice

(https://github.com/osquery/osquery/issues/6582)

The error case is that we see duplicate events in the logs. This means
- Severely degrades performance -- lots of work due to doubling
- Severely impacts data integrity
- Issues scattered around reporting symptoms

No reliable reproduction yet.

Some speculation:
* when Nick removed the windows events from core, the static
  initialization might have started doing something twice.
* In the config change, we _stop_ all the publishers, then _start_
  them. So there might be a bug there.
* If osquery has stopped things, but the OS thinks there are still
  callbacks, when it's restarted the old things may come back.

Some things to look for, if you're in this case:
* Look at the events table, are we seeing doubled publishers or
  subscribers?


## Cloud Tables And EAV

seph is curious if there's interest in a single cloud metadata table
rolling up ec2, azure, and gcp? This would be an EAV style table. No
one seems to have strong opinions, or familiarity with EAV
style. There are some concerns about compatibility with existing
schemas.

## RocksDB

Teddy thinks we might be misusing rocksdb
* There's work to profile it, and understand how it would work best with our events.
* There are a lot of configuration options, we might not have chosen best.
* There might be better data layouts
* Something about transactions
* We maintain indexes for the published content in rocksdb.
  - Storing the indexes in rocksdb may be wrong. Alessandro has
    proposed storing the indexes in ram instead.
  - This should be higher performance
  - Should cut down on database migrations
  - c++ work to do this

We probably need some help here. If someone has some background in
data analysis and is excited to tackle it.

TODO: Make a ticket so the idea is captured here

## Look at old PRs

_(If there's time, we've been trying to re-visit old PRs)_

[Reverse Sorted List of PRs](https://github.com/osquery/osquery/pulls?q=is%3Apr+is%3Aopen+sort%3Acreated-asc)
