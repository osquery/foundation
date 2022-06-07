# osquery office hours 2022-05-10

YouTube Link: https://www.youtube.com/watch?v=L2WF2_MBSro

## Announcements and Highlights since the last meeting

## Any Questions / Issues / PRs people want to discuss?

The intent of this section is to provide a clear time for community members to bring up _anything_.

Broad questions? Bugs? Deployment questions? Blocked PRs?


## What is the best way to continue with an abandoned PR?

seph says that if they've signed the CLA, then it's probably reasonable to fork their code and pick up.

If they have **not** signed the CLA than it's a bit harder. I think we don't have rights to the code, so one would need to make a clean room implementation.

Probably also polite to attribute GitHub co-author credit, and to mention it.

## Releases and PRs?

What's the state of 5.3.0 and 5.4.0?

Some discussion about PRs in flight
* Discussion about openssl https://github.com/osquery/osquery/pull/7581

Let's cut 5.3.0 this week.

5.2.3 is stable, but the web site hasn't been updated.

## NULL vs Empty String

In several places we do things like `r["issuer"] = SQL_TEXT("");` Whether we can use nulls, is remarkably complicated.

While sometimes we can _omit_ setting the key, which will sometimes trickles up to a `NULL`, this ends up with some weird issues when using extended schemas or using the internal query API. The _lack_ of key means the column is missing.

And we cannot just set the value there to a `nil`, because part of the api there is a map of string. 

There have been some expirments around supporting types. But we're not there yet

## EndpointSecurity based FIM:

https://github.com/osquery/osquery/pull/7579

Now builds on 10.15 Catalina too, with path muting using an older API

Some inspiration from Santa. 

Under Monterey, Apple adds a bunch of system paths to the muting list. We will document these, and prior to Monterey people may wish to mute them. 

Please review

## Look at old PRs 

_(If there's time, we've been trying to re-visit old PRs)_

[Reverse Sorted List of PRs](https://github.com/osquery/osquery/pulls?q=is%3Apr+is%3Aopen+sort%3Acreated-asc)
