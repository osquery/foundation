# osquery office hours 2022-03-01

YouTube Link: https://www.youtube.com/watch?v=U8G6_i38FXo

## Announcements and Highlights since the last meeting

* Release 5.2.2 stable. Website to be updated today.
* Zach or Seph will tweet about the new release, after the website's downloads page is updated.

## Any Questions / Issues / PRs people want to discuss?

The intent of this section is to provide a clear time for community members to bring up _anything_.

Broad questions? Bugs? Deployment questions? Blocked PRs?

* https://github.com/osquery/osquery/pull/7489
* https://github.com/osquery/osquery/pull/7485 


## Short discussion about PR expectations

One of the folks asked what PR expectations are. Some discussion:

* We're pretty casual. Pinging people on #code-review on slack can work
* ToB tries not to approve coworker PRs, But they also most of the deep c++ people. The general sentiment is that if Alessandro or Stefano says a PR is good, someone else can do a pretty quick/light review

## How to document table limitations?

Not clear there's a better answer than parenthetical comments in the spec files. 

### running_apps (macOS)

https://github.com/osquery/osquery/issues/1943

`is_active` column in `running_apps` requires a run loop on the main thread

Which means when querying via the `distributed` endpoint, since it's running in a different thread, it doesn't reflect the true data. This is a quirk of `NSWorkspace` API (talked about in older issues).

There is no simple "fix" for this (happy to hear thoughts), but how can we document this better?

Discussion about whether we can fix this issue, or not emit data or what. 

### wifi networks

Similar to above, any process for "deprecating" columns on the table

seph points out there's a magic quoting column hack to allow selecting if columns may or may not be present. (Use double quotes)

Given that this table is broken, and the data in question isn't coming back, we should just remove the columns. 

## Updating the minimum macOS SDK

I want to propose moving from 10.12 (unsupported by Apple since October 2019) to 10.13 (unsupported by Apple as of December 2020) as the minimum SDK.

There's general consensus support for doing that. 10.12 is Sierra, which is quite old. And devices should be able to upgrade to High Sierra. 


## Look at old PRs 

_(If there's time, we've been trying to re-visit old PRs)_

[Reverse Sorted List of PRs](https://github.com/osquery/osquery/pulls?q=is%3Apr+is%3Aopen+sort%3Acreated-asc)
