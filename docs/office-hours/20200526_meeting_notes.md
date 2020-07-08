# osquery office hours 2020-05-26

YouTube Link: https://youtu.be/eUWhmqkued8

## Announcements and Highlights since the last meeting

- We now have an Apple Developer Account, as a non-profit team
- We applied for a CVE for OpenSSL-related issue (fixed in PR #6433)
  - This security adisory was is driven through GitHub. So we have. CVE process outside FB
  - As a note we should backfill these -- https://github.com/osquery/foundation/issues/59

## Issue 4787: Schema consistency question

https://github.com/osquery/osquery/issues/4787

Do we want to separate path and args, in `services` table the way we
do for `startup_items` table? This would be an easy change, we would
be more consistent.

Whatever we do, we shoud make sure we don't confuse the execv path,
from the user controlled named `$0`. This is not a concern on this
table.

This change is breaking change. We still don't have a good answer for
that, but we think it's reasonable in this case.

## EndpointSecurity process events table

https://github.com/osquery/osquery/pull/6467/

Sharvil made a first pass at EndpointSecurity (ES) for a process events
table.

This requires `-DCMAKE_OSX_DEPLOYMENT_TARGET=10.11` which should be
set. But there may be an Azure issue blocking a build.

Some discussion about how to manage the different backends. Does it
make sense to create different tables, or different backends with a
config switch to the same table.
* Sites that want to be explicit about what they're doing, may want to
  see different tables.
* But the process table seems highly dupliciative.
* What do heterogenous fleet managers want to see?
* https://objective-see.com/blog/blog_0x47.html has some discussion of
  how ES and OpenBSM expose things.

Some potentiual caveats about endpoint security.
* There may be a crash in some osx versions. Not yet clear. Alessandro
  and Sharvil are discussing
  it. https://twitter.com/patrickwardle/status/1250337659022532610 may
  be related.
* ES counts as a system extension. Which causes
  downgrades to prompt for user input. (upgrades do not). Groob's
  employer is running into issues here, and they're following up with
  Apple.
* At _runtime_ if the tool tries to load the ES framework, it triggers
  a user prompt. (or an MDM one). So we should ship this disbled.

## openBSM improvements and review

Nishant and Teddy are working on improving the OpenBSM
implementations. They'd appreciate more eyes on
https://github.com/osquery/osquery/pull/6436

Note that we merged auditpipe that allows osquery to adjust the
OpenBSM configuration dynamically. So folks don't have to adjust their
settings by hand when testing.

## Release scheduling

https://github.com/osquery/osquery/milestone/49

seph will move all non-applicable issues from the 4.4.0 milestone to
the 4.5.0 one.

4.3.0 was April 15th, A 2 month cadence would be targetting June 1.

We should discuss accelerating the next osquery release, because of
those security-related OpenSSL updates (Mike).

We should start planning an Apple signing changeover. This would
likely be 5.0 to help anyone who pins the signature to FB.

## Apple Developer Account

We should apply for ES. Since that has a bit of a lead time. We
think this is reasonable to apply to now. TODO: seph click some buttons.

An organization account can support multiple levels of members. We could
add contributors so they can do developer signatures, but they'd not be
able to do distribution. More discussion needed.

## Removing downloads during build

We're close to having osquery be able to build without downloads. This
would allow a process like clone, then a move to build machine, and
build.

The two things that aren't working --
* OpenSSL target downloads OpenSSL. Can we move this to a submodule?
* Part of the python code downloads jinja2. Disussion about how to do
  this. Moving to a submodule seems like the simplest path forward.
