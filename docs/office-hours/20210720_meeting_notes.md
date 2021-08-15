# osquery office hours 2021-07-20

YouTube Link: https://www.youtube.com/watch?v=rvv5o_CZClQ

## Announcements and Highlights since the last meeting

## Any Questions / Issues / PRs people want to discuss?

The intent of this section is to provide a clear time for community members to bring up _anything_.

Broad questions? Bugs? Deployment questions? Blocked PRs?

### Does osquery support logrotation

While osquery's general stance is that log rotation is the responsibility of the underlying OS. **But** as of 4.9.0, osquery does support file rotation.

### Some discussion about load order

If you have a config file that points to an extension provided logger, then that extension needs to be loaded before the config will load. This is as expected.

### Does osquery work without root permissions?

Yes -- osquery itself does not require root access. However, the data osquery has access to is going to be limited to what the user is running as.

## 5.0.0

Blockers? What do we need to release on all platforms?

* macOS paths update (Sharvil to update, seph to review)
* linux paths update (???)
* windows codesigning cert? (on seph's plate)
* Facebook in various profile names (seph will try to open a PR)

## Someone asks about m1 Apple Silicon Support

Someone from Sophos askes about m1 support. Some discussion that it's not on anyone's immediately plate, but that PRs would be welcome. 

This is not something that 5.0 is blocking on, and it is not something that would merit a new release. 

## Some discussion about the performance metrics

It would be neat to get performance metrics for distributed queries.

It would also be nice to have a proxy for the output size. This metric was removed awhile ago because of inaccuracies with serielization size. (Or so says the commits). Number of rows might be a find proxy here. 

These are driven a but by discussion about how data gets lost when it's too big for the destination. Visibility into what's happening would help.


## Look at old PRs 

_(If there's time, we've been trying to re-visit old PRs)_

[Reverse Sorted List of PRs](https://github.com/osquery/osquery/pulls?q=is%3Apr+is%3Aopen+sort%3Acreated-asc)
