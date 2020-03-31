# osquery office hours 2020-03-31

YouTube Link:

## Announcements and Highlights since the last meeting

## Any PRs people want to discuss

The intent of this section, is to allow contributors to bring up and
PRs they feel blocked on.

### Add messages and statements to distributed query results

https://github.com/osquery/osquery/pull/6352

seph asks if this impacts any of the SDKs, extensions, or
servers. There shouldn't be a direct upgrade problem, this does not
touch thrift. But this does change the json responses, so if anyone
has a strict json parsing enabled, it might break that, but we are not
aware of anyone doing parsing like that.

### Implement container access from tables on Linux

https://github.com/osquery/osquery/pull/6209 

Need a review. Blocking PRs #6216 and #6218

The base problem here is that you can't switch namespaces without
forking. So this adds some forking.

There seems to be broad consensus that the approach is reasonable. Or
at least we don't have any other options. We do not believe this will
impact people not using this.

Zach will re-review.

## Implement event batching support in Windows tables

https://github.com/osquery/osquery/pull/6280

ToB had a customer that had issues here. Rocksdb was triggering a
write per event, and was unable to keep up. This caused rocksdb
lockups. This PR changes that model, and adds tests.

The linux and macOS implementations have already been updated. This is
bringing windows to feature parity, it uses the same add batch api
introduced prior.

Some related conversation about rocksdb constraints. In the past we
had relaxed some constraints. As we move the high volumes writes, can
we re-enable those? It sounds like we need transaction support first
-- Sometimes we touch multiple keys, so without transaction support,
inconsistency is still possible.

This, might, change the format on the event logs. There's a new parser
implementation. There are tests now. It sounds like we're not even
sure how the data will look different. What kind of communication do
we need? Discussion about a blog post? Teddy suggests something like
"We're fixing some stuff. Here are some examples. Here's an issue to
comment on". Vern mentions that they're tracking some places where the
existing implemenetion doesn't have some attributes.

Alessandro proposes we remove the database index on events. Instead
that can be kept in ram. This would cut down on the database writes,
and we _think_ also remove some of the errors on DB upgrade. We think
this work can happen separately.

## Agenda down here

## 4.3.0 Patch Release

We've had a couple of notable bugs discovered, and fixed. This merits
a patch release.

* `--check_config` not exiting -- https://github.com/osquery/osquery/issues/6290
* process `cmdline` reporting -- https://github.com/osquery/osquery/issues/6330 

Some discussion about whether we should call this should be 4.2.1 or
4.3.0. We settled on 4.3.0

Windows UTF-8 Issue -- We've merged some PRs related to this
(https://github.com/osquery/osquery/pull/6190), but not others
(https://github.com/osquery/osquery/pull/6338). We do not think this
is a release blocker. Teddy will check with farfella about it.


## Filtering events in subscribers (Zach)

Has anyone done work involving filtering events on the ingestion side in osquery?

* Often, RocksDB is buffering lots of events that will never get processed
* What could a system for pre-filtering events look like?
* Possible/reasonable to do this in event subscribers?

Uptycs has raised some related things in [office
hours](https://github.com/osquery/foundation/blob/master/docs/office-hours/20191029_meeting_notes.md#events-forwarding-uptycs-nishant),
a [PR](https://github.com/osquery/osquery/pull/3482) and a
[blueprint](https://github.com/osquery/osquery/issues/5966)

AlienVault had [PR'ed
something](https://github.com/osquery/osquery/pull/6036) related,
using conveyor. But, this approach makes transactions harder --
rocksdb in theory garentees data integrity. There is also discussion
that the batch changes Alessandro made may make this less required.

PolyLogyx? https://github.com/polylogyx/osq-ext-bin#2-applying-filters

A bit of a sprawling conversation about what the vision of osquery is.
* Are we a sql engine / interface?
* Do we want to just stream events, that gets away from the sql engine
* There are pragmatic arguments
* Practically speaking, you probably can't join usefully anyhow. So maybe we should embrace it
* Maybe there'a some other OSS work around streaming and sqlite.

## AArch64 support

Most of the work has been already done by a contributor (thank you
@artemist!). The PR has been merged in a separate branch, so we can
all help out and solve the remaining small issues.

The branch is located here:
https://github.com/osquery/osquery/tree/aarch64-support

We are testing the build, sometimes re-running the original build
system of some of the libraries on Ubuntu 16.04 to update the
configuration (add/remove missing/unneeded options).

Before this merges to master, we need to update some libraries.
