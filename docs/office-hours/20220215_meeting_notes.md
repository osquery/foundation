# osquery office hours 2022-02-15

YouTube Link: https://www.youtube.com/watch?v=QnkgFnI2le4

## Announcements and Highlights since the last meeting

## Any Questions / Issues / PRs people want to discuss?

The intent of this section is to provide a clear time for community members to bring up _anything_.

Broad questions? Bugs? Deployment questions? Blocked PRs?

### How to handle errors in table

If a generate function can't parse data, what happens?

Generally discussion is that we don't have clear standards. Returning no data and logging on either info or verbose, depending. Sometimes we return a `-1` sentinal value. 

## Agenda down here

## Error Handling

How do we think about error handling? In some places osquery we have this `Expected` class, but it's not universal. 

The `Expected` class is the new way, ideally everything should be using it. 

What about `check` or `assert`? What about using them in debug context? ("`dcheck`"). General sentiment is that asserts are an ill-fit for the stable long running process model. But what about debug asserts? Probably we've never thought about or felt the need for them.


## 5.2.2, is it stable?

https://github.com/osquery/osquery/releases/tag/5.2.2

seph has been running it in Kolide's beta hosts. No issues.

Sounds like no one has seen issues.

Let's call it stable!

## Watchdog timeouts

https://github.com/osquery/osquery/pull/7474

If you throw a lot of data at Yara, it will consume a lot of memory very rapidly. The watchdog gives osquery a fair bit of time to shutdown. But, this watchdog timeout means osquery can consume entirely too much ram.

So should the watchdog shutdown osquery faster? The tradeoff is that this doesn't let osquery shutdown cleanly.

There is added complexity here, because the same timeout is used for watchdog timeouts _and_ for normal shutdown. Eg: if systemd triggers a shutdown, there's a graceperiod before the daemon must exit. So we implement an internal grace period. This period is the same as the watchdog one. 

Eg: Tradeoff between catching runaway processes and letting osquery shutdown

This data was changed recently from 4s to 15s.

Some discussion about what the purpose of machine is. The purpose of a machine is probably to run workload, osquery should not get in the way of that gracetime.

Maybe just split the timeout settings? And make it configurable? Not a great answer, but it seems hard find somethjing perfect.

## Etiquette/Convention on PR (approved) merging

Basic expectations:
1. If developers have permissions, they should merge themselves
2. Try not to merge in the middle of a release.
3. Try to have someone outside your employer do the review/approve



## Look at old PRs 

_(If there's time, we've been trying to re-visit old PRs)_

[Reverse Sorted List of PRs](https://github.com/osquery/osquery/pulls?q=is%3Apr+is%3Aopen+sort%3Acreated-asc)
