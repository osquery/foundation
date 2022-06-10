# osquery office hours 2021-12-07

YouTube Link: https://www.youtube.com/watch?v=yLCRtrhDGZI

## Announcements and Highlights since the last meeting

### 5.1.0 Released

We released https://github.com/osquery/osquery/releases/tag/5.1.0 This was primarily to fix an BPF resource leak.

Probably needs to be pushed to chocolatey.

### Sharvil is joining FleetDM

Sharvil is joining FleetDM to work on osquery. (Congratulations!) Notable to us, is that if there are things around code review or core osquery 
work, Sharvil is likely to have more time for it. 

## Any Questions / Issues / PRs people want to discuss?

### Discussion about osquery and containers

Roxana is expirmenting with running/compiling osquery inside a container. It sounds like she probably is looking for a linux binary distribution

## Agenda down here

## Odd behaviors seph noticed

seph ran into a weird issue on his local machine where the `apps` table started hanging. This ended up looking very weird -- a distributed query would try that, and hang, but the scheduled queries would keep going. This might be a change sometime in the 4.x line. And to be clear, this is weirdness on seph's machine, not osquery. 

Also seen some things on windows where if there are many launcher/osquery pairs running, some will not work. Stefano suggests there may be some lock contention on windows. A lot of file open stuff uses exclusive locks. Something to poke around. 

## 5.2.0

AKA: Please review the [big m1 pr](https://github.com/osquery/osquery/pull/7330)

## Discussion about older platforms

There are some things that slowly make it hard to support old platforma
* newer libraries
* third party services with unversioned wire prototols
* Kernel headers

This we eventually need to drop support for old platforms. Some discussion about what triggers this. Some discussion about whether there's interest in a paid long term support option.

We should probably have better testing and more clear statements about what we support. 

We think we can probably drop support for centos6

## Adding non-blocking socket support to the bpf_socket_events table

Issue link: https://github.com/osquery/osquery/issues/7408
Audit implementation (merged): https://github.com/osquery/osquery/pull/7269

## Look at old PRs 

_(If there's time, we've been trying to re-visit old PRs)_

[Reverse Sorted List of PRs](https://github.com/osquery/osquery/pulls?q=is%3Apr+is%3Aopen+sort%3Acreated-asc)
