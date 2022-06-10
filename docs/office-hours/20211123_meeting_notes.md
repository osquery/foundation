# osquery office hours 2021-11-23

YouTube Link: https://www.youtube.com/watch?v=zQVISlOKi9s

## Announcements and Highlights since the last meeting

* The BPF memory leaks fix has been merged: https://github.com/osquery/osquery/pull/7302
* Apple M1 support is ready for review: https://github.com/osquery/osquery/pull/7330

## Any Questions / Issues / PRs people want to discuss?

The intent of this section is to provide a clear time for community members to bring up _anything_.

Broad questions? Bugs? Deployment questions? Blocked PRs?

### Some discussion from RD

A new user, and she had some questions. we're talking about about kind of an overview, with some focus on events.

### Are people paid to work on osquery?

osquery itself is a non-profit and doesn't pay people. But several of the people involved are paid by their employers. 

## Release? 5.1.0

There's still some interested. Plan is to cut a 5.1 today, leave it in pre-release until seph can make CHANGELOG. 

Things we didn't do:
* Think about how to have the 5.x line remove stale 4.x 
* Linux should have had symlinks into `/usr/bin` let's try to do this (https://github.com/osquery/osquery-packaging/pull/18/files)

## Apple M1 Support

https://github.com/osquery/osquery/pull/7330 is ready for review

Commits are split, and grouped. So it should be reviewable by commit.

After this merges, we can start looking at the build/package side.

## Small side discussions


* Side discussion about inbound vs out-of-band error reporting. What would it be like to have ab `errors` column?
* Is it possible to get nightly builds available to non-logged in users
* Is there a table for returning the osquery config? No... Maybe there should be? Does anyone have secrets there? What about YARA urls?
* Why do we have both `disable_carver` and `carver_disable_function`? The UX is weird?

## Look at old PRs 

_(If there's time, we've been trying to re-visit old PRs)_

### [Fix Unified Access Logging parser on](https://github.com/osquery/osquery/pull/7259)

We should ensure our constraint handling is correct. But if the underlying API is surprising, that's what it is. 

If someone queries this with no constraints, we should not return so much data we oom. eg: query without constraints should either throw an error, or have some kind of limit. Likely better UX to have a mandatory WHERE clause, so people don't think the limited output is all there is.

[Reverse Sorted List of PRs](https://github.com/osquery/osquery/pulls?q=is%3Apr+is%3Aopen+sort%3Acreated-asc)
