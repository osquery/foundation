# osquery office hours 2020-12-22

YouTube Link: https://youtu.be/GbXi6xR7gZE

## Announcements and Highlights since the last meeting

### 4.6.0 Shipped!

Smooth sailing! This one came out of CI, so that's nice. Changelog is simpler. Slow improvements everywhere.

### Nick is back at Facebook

We expect to see more of him again. Yay!

### Zach is speaking at osquery@scale!

Announcement tweet: https://twitter.com/fleetctl/status/1340054821273968640

## Any Questions / Issues / PRs people want to discuss?

### Discussion about archtectural things

Danny is an SRE at Grafana, and he's curious about what osquery and prometheus might look like. There's an exporter someone is playing with. And he's curious if people have done this before, or what. 

Some discussion about metrics vs logs.

What about a different direction -- osquery itself could have some performance metrics, and those could be exposed to a prometheus exporter. 

Some potential followups
* https://github.com/jupp0r/prometheus-cpp
* https://github.com/zwopir/osquery_exporter


### Discussion about site settings as flags vs settings

Coming out of a [Slack thread](https://osquery.slack.com/archives/C01DXJL16D8/p1608315598376200?thread_ts=1608314695.372300&cid=C01DXJL16D8)

Some options are supposed to be flags only, but this might not be correctly enforced.

Flags are _intended_ to define the startup behaviors. Whereas options are intended to be dynamic. Remember that config files are, themselves, a option specified plugin.

One difference is that TLS servers can set config files, but not flags.

However, the config file is parsed _after_ the flags. This means the server can override flags with a conf. 

## 4.6.0.2 Release

The motivator is to change the windows linking to static. As we understand it, the hosted 4.6.0 is probably not widely compatible. https://github.com/osquery/osquery/pull/6818

We can probably just rebuild with a different CMAKE options. But would be a bit weird, since we don't have release numbers in darwin or windows. (https://github.com/osquery/osquery/issues/6837)

Tagging a 4.6.1 might bring in undesired changes. We could cut a release branch, and cherry-pick. 

Our consensus is that we should build a windows `4.6.0.2` release for this. This is a manual build time option to a straight 4.6.0 build.

## CI / Release improvements

A short conversation. Still places to improve

* macOS codesignging
* linux needs debug/release artifacts
  - This might ben a new job. With debug info, but without tests

MacOS CI is now running on catalina

## Big Sur and M1 oddities

### os version numbering

https://github.com/osquery/osquery/pull/6824 there's a real issue here, though this PR might not have fixed it. Discussion is that maybe the env is only checked on application startup?

We're going to keep using an old SDK...

### nvram table

Kolide reports that we see some failures on the nvram table. The data is there, but using a `WHERE` clause sometimes it's no longer returned. No great test case yet though. 

Some theory that maybe this is coming in as `CFData` and we don't always parse those correctly. 

## Thrift upgrades

https://github.com/osquery/osquery/pull/6822

Teddy and Mike are working on upgrading Thrift.

Several drivers:
* We should be closer to current
* Newer thrift allows us to instantiate pipes with better security settings
* Might fix the ping errors as well

## Look at old PRs 

_(If there's time, we've been trying to re-visit old PRs)_

[Reverse Sorted List of PRs](https://github.com/osquery/osquery/pulls?q=is%3Apr+is%3Aopen+sort%3Acreated-asc)
