# osquery office hours 2020-11-10

YouTube Link: https://www.youtube.com/watch?v=sFCdAmpYU2I

## Announcements and Highlights since the last meeting

### FleetDM and the new home for Fleet

Mike and Zach are here!

There are no immediate direction changes. Plan is to keep making it a great tool. Easier for widespread adoption. 

Business model is going to be Open Core. 

Goal of trying to make osquery a richer and more full featured agent. Discussion about how to bring in vulnerability management, and what other data sources might look like. 

## Any Questions / Issues / PRs people want to discuss?

The intent of this section is to provide a clear time for community members to bring up _anything_.

Broad questions? Bugs? Deployment questions? Blocked PRs?

## Updating sample extensions

The sample extensions that are present in the repository are often out of date and not buildable. [PR6747](https://github.com/osquery/osquery/pull/6747) moves them inside the **external** folder, so that we'll more easily notice when they break.

## Extensions writing to the database

Some extensions saved some things to the database. This presented problems:
* There is no interface forcing developers to version data formats, potentially causing issues when extensions are updated
* There is no uninstall procedure built into osquery that also cleans up data left in the database by extensions
* It encouraged bad practices, changing the osquery database
* Unless write batching is used (or it's used in a bad way) it will cause performance issues
* Since RocksDB transactions are not implemented, it's easy for data to get corrupted when saving multiple related keys

It may not be easily decoupled today

TODO: seph: Make an issue

## Huge refactor of the eventing framework in progress

We have a big refactor in the works, that removes indexes from the RocksDB database. This should improve performances and data integrity for all publishers. Will post test packages in core [PR6610](https://github.com/osquery/osquery/pull/6610)

## Apple Announcements today

* arm64 machines shipping this month
* macOS 11 release? Thursday Nov 12

A handful of people are running Big Sur betas (Kolide, Sharvil). No one has exhaustively tested all the tables, but no problems have yet been observed.

Same story for arm64 -- Seems to work.

## systemd support

Originally implemented in [PR4019](https://github.com/osquery/osquery/pull/4019), systemd functionality has been reimplemented using dbus. These dependencies have been cleaned up, and these now need review.

 - [PR6562](https://github.com/osquery/osquery/pull/6562): Add systemd to **startup_items**
 - [PR6593](https://github.com/osquery/osquery/pull/6593): Add **systemd_units** table

## extended_attributes for Linux

[PR6195](https://github.com/osquery/osquery/pull/6195) ports the **extended_attributes** to Linux, also adding support for decoding capabilities using libcap.

## Windows Extensions

Some reports of errors. Mostly limited to windows. Might be several different bugs. 
* The socket can timeout, and then repeat.
* Might be related to the "TPipe GetOverlappedResult failed"
* Sometimes we see output become garbage. Looks like a bunch of intermingled output

The `GetOverlappedResult` isn't really new. But we used have thrift mask those errors, we changed that in 4.x.

We're using thrift 11 (not 12), and probably haven't regenerated the bindings recently. 

## Registering 2 extensions can deadlock

Issue 6744 observed that opening two extensions can sometimes cause a deadlock. A fix, 6745, was submitted as well, but there is a desire to refactor a bit and fix some of the locking. Teddy thinks we should refactor this to have a mutext per private data. 

Some discussion that the mutexs may be too fine grained. They protect fields, but perhaps they should instead be more transactional?

## Transferring osquery-go to the Foundation

https://github.com/osquery/foundation/issues/65

As there's a majority of TSC quorum at this office hours, seph raised the issue around transferring osquery-go to the foundation. There's broad consensus for this, Kolide is on board, but the MIT license is not something we just can accept. Our charter mandates a different OSS license. We can, however, accept this with a majority TSC vote.

The choices are to leave it in Kolide until it can be relicensed. Or to transfer it and pursue relicensing it under the osquery org. seph thinks it's better to accept it as is.

There were 4 TSC members present (seph, Teddy, Zach, Alessandro) and all voted in favor. None opposed

## Review Old PRs

_(If there's time, we've been trying to re-visit old PRs)_

[Reverse Sorted List of PRs](https://github.com/osquery/osquery/pulls?q=is%3Apr+is%3Aopen+sort%3Acreated-asc)

### File Events [pr6663](https://github.com/osquery/osquery/pull/6663)

The core of this is that there are two conf options that can start up FIM events. This creates some new flags for it. 

At some point, we're going to need to think about how to manage this abundance of flags. 