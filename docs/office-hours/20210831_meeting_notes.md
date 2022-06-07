# osquery office hours 2021-08-31

YouTube Link: https://youtu.be/Wt0EPRpbFl0

## Announcements and Highlights since the last meeting

* 5.0.0 pre-release available

## Any Questions / Issues / PRs people want to discuss?

The intent of this section is to provide a clear time for community members to bring up _anything_.

Broad questions? Bugs? Deployment questions? Blocked PRs?

### Puffycid requests PR review

They've got several PRs open. Please review!

## Question about the File table and recursion

This stems from a [slack conversation](https://osquery.slack.com/archives/C08V7KTJB/p1630358095015700?thread_ts=1630357589.015200&cid=C08V7KTJB) trying to recurse through the file system table. There's a depth limit there. Is this intentional?

Stefano doesn't think there's a hardcoded limit.

There's some indication that there's a [max recursion set to 64](https://github.com/osquery/osquery/blob/master/osquery/filesystem/filesystem.cpp#L52). Unclear whether the code works as implied. 

There is likely platformm skew here as well. The windows glob implementation is known to be slow. 

Mike McNeil is digging into this a bit. See https://github.com/osquery/osquery/issues/7291

## CA bundle

The current CA bundle is over 2 years old (apparently from Mozilla). Should we schedule a reminder to update it once in a while? It could be added to the release checklist

seph raises a side question about system certificate stores. But thinks that we cannot just use the system stores withouth some additional code to translate between the formats.

Some consensus that updating this is fine, and that updating it more often is also fine. No volunteers to do that. (GitHub actions anyone?)

## 5.0

We cut 5.0.0 last week, and unsurprisingly bumped into some issues. Where are we with 5.0.1

There have been some fixed plist issues, and systemd issues

Expecting a 5.0.1 today


## macOS 12 Monterey 

socketevents depends on openBSM. Rumor is this may be removed in monterey. Still just rumors...

We will probably need to generate a new provisioning profile, but we don't think we need more permissions from Apple. 

## Apple Silicon M1

There are some issues/milestones. 

seph has started looking at CI for this. Still unknown.

How do we feel about cross compilation? Very basic tests are successful. seph thinks there are two questions. First, is the build fast enough. Second, do we need real m1 machines to test?


## Look at old PRs 

_(If there's time, we've been trying to re-visit old PRs)_

[Reverse Sorted List of PRs](https://github.com/osquery/osquery/pulls?q=is%3Apr+is%3Aopen+sort%3Acreated-asc)
