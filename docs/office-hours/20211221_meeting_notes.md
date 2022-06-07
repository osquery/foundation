# osquery office hours 2021-12-21

YouTube Link: https://youtu.be/zHM-NOcGtTw

## Announcements and Highlights since the last meeting

* osquery 5.1.0 in pre-release since Dec 3

## Any Questions / Issues / PRs people want to discuss?

 * Shoutout to [@aleksmaus](https://github.com/aleksmaus) for the excellent work with the new [Windows Firewall Rules table](https://github.com/osquery/osquery/pull/7403). I think this implementation should be used as reference for all the upcoming Windows-related PRs - Alessandro
 * 

## 5.1.0 Release

Is this stable? Has anyone tested it?

* Kolide has pushed it to beta, no issues yet.
* Shavil has done some testing
* Maybe some of the BPF people have tried

No reason not to push this out to the world

## 5.2.0 Release

[Apple Silicon](https://github.com/osquery/osquery/pull/7330) 

Kudos to seph and Sharvil for going over this PR

Before this can be merged, we have to relax the CI requirements since the GitHub Actions configuration has been changed by moving the workers around. This is because the CI workers are current catalina, and we're moving it to Big Sur, so there's some coordination for merging. General consensus is that we can be pragmatic -- turn off CI, merge, update the CI settings. As long as we're running again soon.

Then, open an issue to track a better fix after the holidays.

[Windows Firewall Rules](https://github.com/osquery/osquery/pull/7403)

What's our drop date? Aiming for by the holiday

Sequencing:
1. turn off CI requirement on PRs
2. merge #7330
3. fix CI
4. Fix packaging

## Blueprint to change Windows uid, gid interpretation and performance

https://github.com/osquery/osquery/issues/7417

This started because joining users to groups on windows, using gid. But windows doesn't use gid, so the join is an inadvertent cross join. 

The existing uid (and gid) isn't a real windows concept. It's a fragment of the full SID. Stefano is proposing _changing_ the windows uid and gid to be SIDs. This would be more correct, and more compatible with how windows work.

Existing queries should keep working -- All the values would change together. 

This would be a _breaking_ change for any consumers of this data. And that it would potentially be quite wide ranging -- UID and GID shows up in a log of places (file, processes, etc)

Some discussion about performance of queries, tables, CTEs, and exposing the underlying table costs to the query planner. 

Support for this has waxed and waned. 

## Blueprint to improve row filtering performance
https://github.com/osquery/osquery/issues/7420

Some side discussion about NULL vs empty strings. And how this is handled on the backend. 

## Discussion about creating reference tables for future developers

We have a lot of history and a lot of tables people have written. This means that when people go to implement a table, sometimes they pick the wrong example. We should designate some reference tables. Alessandro is recommending https://github.com/osquery/osquery/pull/7403 as one. 

We should make sure we have a good "Creating new tables" wiki page, and we should make sure that it links to good example tables. 

## Platform specific columns

Stefano noticed, and brings up the the column definitions in the spec. The existing `extended_schema` mechanism does not work wholly as expected -- it creates all columns everywhere. But marks them as hidden on the other platforms. This is probably not what is expected.

## Look at old PRs 

_(If there's time, we've been trying to re-visit old PRs)_

[Reverse Sorted List of PRs](https://github.com/osquery/osquery/pulls?q=is%3Apr+is%3Aopen+sort%3Acreated-asc)
