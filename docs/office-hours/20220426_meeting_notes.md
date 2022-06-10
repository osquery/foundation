# osquery office hours 2022-04-26

YouTube Link: [https://www.youtube.com/watch?v=KK7Ob_8mVvE](https://www.youtube.com/watch?v=KK7Ob_8mVvE)

## Announcements and Highlights since the last meeting

(none this week)

## Any Questions / Issues / PRs people want to discuss?

The intent of this section is to provide a clear time for community members to bring up _anything_.

Broad questions? Bugs? Deployment questions? Blocked PRs?

## Finalizing 5.2.3

- Merging a changelog for 5.2.3: want to merge to master, not to a release branch
- Marking it a release (it has been in pre-release 20 days)
- Updating the website, package servers, etc.
- Fleet has run automated smoke test (this is pretty limited)
- During the meeting, we made 5.2.3 the release. Changelog and website updates will be handled after the meeting.

## Marking a 5.3.0 to finish April

https://github.com/osquery/osquery/milestone/62

- PR #7350 is a small PR from ToB to mitigate a hypothetical DoS case; it has been open a while and reviewed some already. Longer-term follow-ons to this are planned for later https://github.com/osquery/osquery/issues/7565
- PR #7516 is a more involved PR from ToB to greatly improve performance around the users and groups tables, which fare poorly on Domain Controllers — a complaint that has come up more than a few times in our Slack in recent months. Can we prioritize a review of this? (Thanks to Sharvil who provided a review; we merged this during the meeting)
- PR #7507 -- determine whether processes are translated (running under Rosetta) on macOS https://github.com/osquery/osquery/pull/7507

## Discussing the process for code review

Given the small number of people who review PRs, is it sustainable to continue waiting for an expert in, e.g., "C++ on Windows" from another organization to review a PR from your organization? It seems the PR backlog is growing. -- MM, ToB

There's general sentiment that we don't have a current problem here, and we do not need formal policy to create structure here. 

## PR Backlog

We have a bunch of stale PRs. They're not seeing work. Sentiment is that if we don't get them merged for 5.4, we should consider just closing them.

## Introducing the bpf_network_events experiment

What it is: a new lightweight BPF-based table that can trace network events

Compared to bpf_socket_events:

 * Lower CPU and memory usage
 * Always provides both the local and remote ip:port columns
 * Container aware (Docker)

How it works:

 * Does not trace system calls (low cpu/memory usage)
 * Traces internal kernel functions to gather the events
 * No runtime dependencies (except kernel version)
 * Uses the BTF debug symbols from the kernel to generate BPF code at runtime that is able to read kernel data

Where to get the PoC and how to use it

 * The build has been published in the bpf Slack channel
 * Start it with: `sudo ./osqueryd -S --verbose --enable_experiments=true --experiment_list=bpf_network_events`
 * Query the `bpf_network_events` table

Requirements:

 * CentOS/RHEL: version >= 8, with kernel >= 4.18
 * Other distros: kernel >= 5.8

Current limitations:

 * Only supports IPv4
 * TCP only

Future work:

 * IPv6 support
 * UDP support
 * New columns: timestamp, community id (Zeek), parent process id, absolute binary path

Huge thanks to Ben Bornholm from [holdmybeersecurity.com](https://holdmybeersecurity.com) for the help with testing

## ES based FIM on macOS

Few design updates from last office hours:

There is a second publisher and a subscriber to handle FIM events separately from ES Process Events. This is different from the 1 Pub: N subcriber pattern. There are a few key advantages to this in this context:

* Better performance and reduced load on the host (FIM can be a firehose, and watchdog could kill it)
* Ability to independently manage path muting for both `es_process_events` and `es_process_file_events`
* Previous functionality of `es_process_events` continues to work transparently

Some caveats: path muting works best on Big Sur and Monterey, additional runloop, a bit of duplicated code and apple has a `ES_NEW_CLIENT_RESULT_ERR_TOO_MANY_CLIENTS` as one of the error (but we haven't breached it). Future ES tables will need another publisher.

Side question about how to debug `ES_NEW_CLIENT_RESULT_ERR_TOO_MANY_CLIENTS`, if macOS returns that error, how can an administrator debug it?

## Look at old PRs 

_(If there's time, we've been trying to re-visit old PRs)_

[Reverse Sorted List of PRs](https://github.com/osquery/osquery/pulls?q=is%3Apr+is%3Aopen+sort%3Acreated-asc)
