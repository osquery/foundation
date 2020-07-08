# osquery office hours 2020-05-12

YouTube Link: https://www.youtube.com/watch?v=GYkw__GuV8Q

## Announcements and Highlights since the last meeting

* Nick left FB, he's at Apple now. But hopefully we'll still see him.

## Any Questions / Issues / PRs people want to discuss?

### Any work being done on the macOS new security framework

seph says there's a codesigning entitlement. It's in the Apple
Developer account backlog. ToB has some related work in this space. So
we can use what they've learned about the entitlements side.

Sharvil made a PoC using this. He'll try to clean it up and PR it

### Windows Eventing

https://github.com/osquery/osquery/pull/6280

Looks good to Nick. Small change suggestions, but it's
mergeable. Alessandro should look at the suggestions and either take
them, or just merge it and iterate.

### Container access tables

- https://github.com/osquery/osquery/pull/6414
- https://github.com/osquery/osquery/pull/6413

There is somewhat blocked by
https://github.com/osquery/osquery/issues/6428 which is a discussion
of column naming, and related meta issues. Discussion about how to
have consistent table APIs.

Comments that we could ask sqlite consortium to add a feature to make
hidden columns not hidden if in the where clause. Or to let the
virtual table edit the hidden column bitmask. In the past, osquery has
gotten some features in sqlite. The turnaround can be pretty fast
though there may be development costs.

We should set the columns to hidden, and then we can merge #6414 and #6413. 
We can do that before we resolve #6428.

## User Survey (Two year recap)

Lauren is starting a two year revisting of her "How Teams are using
osquery" she did at the first querycon.

She's looking for people who have osquery deployed on their fleet
who'd like to talk.

If there are questions we'd like answered, let her know.

## Proposed Table Ideas for Discussion (Mike / ToB)

(Potential work from a client)

### Windows Event Logs but in a non-evented table

There's general interest, as long as there's an API that has query
parameters. We want to avoid having to dig through the files directly.

There is some prior precedence in the `asl` for macOS. Now deprecated,
but it used to use the mac's query stuff.

Nick mostly wrote this a while ago, and it may just need to be rebased
and updated and reviewed:
https://github.com/osquery/osquery/compare/master...muffins:windows-event-log-vtable

### YARA table improvements

Both adding Windows support for YARA, and then avoiding on-disk
artifacts. So, changing or adding a table that could avoid requiring
putting yara sigs on-disk on the host. Example: `select * from yara
where path = '/foo/bar' and sigstring='abcd'`

These were once floated in #5285, but that happened during the broader
code freeze. https://github.com/osquery/osquery/pull/5285

It might also be worth looking at porting `yara_events` to Windows, to
work with the recently added `ntfs_journal_events` table.

## Windows build & codesigning?

Today, the windows build and codesign is manual, mostly done by
Nick. Is this impacted by his departure from Facebook?

The build and package stuff is in PR, so others should be able to
build.

Codesigning is with Nick's personal cert, so that should last until
cert renewal. He thinks he'll still have time to codesign.

Unlike macOS (where people sometimes hardcode bundle IDs) we don't
think there's any downside to changing who signs windows
packages. Though windows does have some reputation building on new
developer accounts.

Chocolatey is a windows package management system. Right now Nick and
seph are listed as osquery maintainers.

## Should we allow osquery to configure openBSM?

On Linux, osquery can autoconfigure the audit rules. openBSM has an
API that allows rule changes, so we _could_ do something similar.

If someone wanted to pick up this code, the difficulty is mostly
around handling rule merges with existing ones. There is well
configured standards in the linux code base.

## osquery behvaior around the watchdog

We have often seen confusion about what osquery is doing. This often
comes from confusion about what actions the watchdog is taking. We've
had some discussion, and we don't think it makes sense to change the
watchdog to default disabled.

Some discussion about how there isn't a communication channel between
the watchdog and the worker. So while the worker tries to understand
what it was doing when it was killed, it's imprecise.

ToB mentions client concerns about tracking performance _outside_ a
query. Most of the tooling is about query performance, very little
about osquery itself.

One thing to do, is to revamp the documentation. It could gain a
section on investigating performance impact.
