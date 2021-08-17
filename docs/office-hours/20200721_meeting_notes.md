# osquery office hours 2019-07-21

YouTube Link: https://youtu.be/mEuNaqHi-zM

## Announcements and Highlights since the last meeting

## Any Questions / Issues / PRs people want to discuss?

The intent of this section is to provide a clear time for community
members to bring up _anything_.

Broad questions? Bugs? Deployment questions? Blocked PRs?

## 4.5.0

Yes, there is a milestone. We've soft targeted September 1st.

## What should a SystemD table contain

ToB would like to add a SystemD virtual table on Linux, based on the
dBus API we've used in the startup_items PR. It would replace our open
Services table PR, but, what SystemD information would be most
valuable to capture? We'd like opinions here and then we will open a
blueprint issue.

Something for systemd units (eg: services) seems pretty obvious. Are
there other systemd things?

Unit files (eg services) should include the status. Presumably this
will be a pretty straight view of the dbus api.

## Big Refactors

We have two pending items that will require a pretty major
refactoring. The first is moving the include files around
(https://github.com/osquery/osquery/pull/6557) the second is updating
the copyright headers
(https://github.com/osquery/foundation/issues/32)

We're going to target the week of August 3rd.

Some big takeaways:
1. Existing PRs will become harder to merge. Get stuff in soon
2. We want to minimize the time between these. This will cut down on
   people's pain.


## Windows 32-bit

https://github.com/osquery/osquery/pull/6543

Do we want this? There's some interest. There's been windows 32bit
machines for sale in the last handful of years.

"Missing parts" include:
- A way to enforce the casting.
- A way to test in CI.
- Small refactors to publishing packages (or we ignore publishing
  32bit packages).

We think our CI platforms allow cross building to 32bit. But it's not
clear we can test it natively. That might be good enough to start.


## Check in on longer term efforts

### ARM64

Two driver -- Apple Silicon; General AWS Support
 
seph has not heard from apple WRT Apple Silicon Dev Kit :(
 
No real progress answering the CI question, which we think we'd
need. There might be an azure pipelines agent here. This should be a
smallish addition to the pipelines.

### Package publishing

seph was looking at using github actions to drive this. But hasn't
looked at this in the last two weeks

### GitHub actions as an Azure CI alternative

seph was looking at using github actions to drive this. But hasn't
looked at this in the last two weeks

Azure probably has more OS support. But seph think it's a lot harder
to work with.

Alessandro has seen some stablity issues with GitHub actions. Workers
disappearing, and dropped webhooks. Neither of these is blocking, but
FYI

### System toolchain support

This is a dependency layer for system distributed libraries. This is
something Arch linux would like

Alessandro had the most context on this, but he's not focused on it
right now.

Someone could take the existing arch work, and try to make a simple
case.

The goal of this would make the arch maintainers life be easier. We're
in an awkward state right now.

### BPF tables

Alessandro is polishing his bpf library, and currently it is his top
priority.

### LGTM integration

There is some disruption due to the LGTM failures.

This has probably been replaced by GitHub Actions.

seph will remove from the PRs. (should be done now)

We should _also_ land the improvements for it.

### 'swappable' event publisher, for example OpenBSM with EndpointSecurity

Sharvil has a PoC for both swappable, and separate tables

There is probably value in this kind of swappability. But we'd need a
pretty clean abstraction for it

This is akin an earlier conversation about a `services` table vs a
`systemd` table

Are the backends really a 1:1 replacement for each other, no. So how
to we represent that skew.

From a code perspective, we're really swapping the subscriber. A
publisher can publish to multiple places.

Lots of discussion about what the right abstractions are. A couple
suggestions about maybe having a specific table with lots of details,
and then a generic table that does some union of the two

If we want to merge code, in a low impact way, it would need to be in
a dedicated table. That defers the larger questions about how to join
data.

Some discussion about whether or not we should be an system
extension. We don't think we need it. Maybe we should apply to do it
later?

Next steps... We should look at Sharvil's PR.

## Look at old PRs 

_(If there's time, we've been trying to re-visit old PRs)_

[Reverse Sorted List of PRs](https://github.com/osquery/osquery/pulls?q=is%3Apr+is%3Aopen+sort%3Acreated-asc)
