# osquery office hours 2022-05-24

YouTube Link: https://youtu.be/Dkt2q9otYOE

## Announcements and Highlights since the last meeting

* Mo Zhu, new Fleet person is doing product work on their agent

## Any Questions / Issues / PRs people want to discuss?

The intent of this section is to provide a clear time for community members to bring up _anything_.

Broad questions? Bugs? Deployment questions? Blocked PRs?

## osquery 5.3

https://github.com/osquery/osquery/milestone/62

- The security monitoring can move to 5.4
- Some simple looking / approved things from Stefano should merge
- Some neat new bigger things, probably in 5.4

## Pack Deprecation (seph)

https://github.com/osquery/osquery/issues/7592

Zach is in favor of not shipping them and documenting them, but leaving them in the osquery repo. (Mostly from a monorepo perspective, why move them)

Stefano thinks similar -- We should stop shipping them. No thoughts about where to put them

There's 4 people in favor of _not_ shipping the packs, and no one wants the status quo. 

## `password_policy` table on macOS

https://github.com/osquery/osquery/pull/7594, ready for review, for 5.4

* mimics `pwpolicy`
* No private API calls. 
* Works with MDM or not
* Queries the OpenDirectory API
* Works across macOS versions

## macOS Unified Logging

https://github.com/osquery/osquery/pull/7598 looks like this is ready again for a review, for 5.4

Discussion about setting default limits, and mechanisms therein. 

If you do a bare `select *` it retrieves logs from the _last_ retrieval until now. So semantically, it's a bit similar to events. A caveat here is that if you have a scheduled `select *`, and you issue a distributed one, the latter will adjust the pointer.

Both Zach and seph think this might be a little weird? It's shifting expectations about queries, and behavior. Some discussion about edge cases here.

There are really 2 things we're talking about:
1. How do we make the naive use case not hurt. Returning no data is a common pattern
2. How do we facilitate someone wanting to use osquery to extract _all_ logs. Likely across many invocations of a scheduled query

There's some discussion about wanting to use this kind of function to extract all the logs. But that might be a better fit for evented tables. Maybe some kind of runloop pushing to an event API. But making an evented table, would mean duplicating the system log stores into osquery storage. Not ideal.

Brainstorming some other ways to get all the data, psuedo-event-mechanism:
* Just using an event table feels like a lot of weird duplication
* What you used diff queries with a time period slightly overlapping schedule
* What if we used a sentinal value? There's some support for this, it seems to make sense. (expanded below)

The idea of sentinel value is that we want to support something like `select * from unified_log where timestamp > 'LAST_TIMESTAMP'`. And the generate function would track the value of last_timestamp. There's broad interest in this. Some questions:
* Would this sentinal value be the singlular last access? (note that `count(*)` would confuse it)
* Would we use a string, or `-1` to preserve the typing?
* Would we support different tracking for different queries? If so, how?
  - let people specify arbirary sentinal values and do magic?
  - track the constraints?
  - TBD!

some discussion about max_rows -- instead of a `FLAG`, what about a hidden column? This is probably needed, since any kind of last timestamp tracking would be confused by sqlite applying a limit.

Conclusions:
* The magic behavior on `select *` is not wanted.
* max_rows should be a hidden column
* having some kind of sentinal value that tracks the last returned timestamp seems desirable
* if max_rows has a default between 100 and 1000, a bare `select *` seems great.


Side note -- this seems like a new idiom for us! Exciting!

## macOS ES based FIM

https://github.com/osquery/osquery/pull/7579

Please review, have been addressing feedback, for 5.4

# macOS kernel_panics table restored

https://github.com/osquery/osquery/pull/7585 it's annoying to test but I have tested it widely - MM

## Look at old PRs 

_(If there's time, we've been trying to re-visit old PRs)_

[Reverse Sorted List of PRs](https://github.com/osquery/osquery/pulls?q=is%3Apr+is%3Aopen+sort%3Acreated-asc)
