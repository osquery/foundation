# osquery office hours 2020-08-18

YouTube Link: https://www.youtube.com/watch?v=N3LpbQ3Fs8c

## Announcements and Highlights since the last meeting

### Big file-touching PRs done

Teddy and seph merged several PRs that touch most files.
* https://github.com/osquery/osquery/pull/6557 moves the include files
  to cmake targets
* https://github.com/osquery/osquery/pull/6589 and
  https://github.com/osquery/osquery/pull/6590 update the copyright
  blocks on all the files

If you have pending work, this may produce merge conflicts. You will
will need to rebase.

## Any Questions / Issues / PRs people want to discuss?

The intent of this section is to provide a clear time for community
members to bring up _anything_.

Broad questions? Bugs? Deployment questions? Blocked PRs?

### ToB PRs

ToB has some PRs in lingering states. Please review!

* Windows Events -- https://github.com/osquery/osquery/pull/6563
  - Zach will look
* YARA -- https://github.com/osquery/osquery/pull/6568
  - I think this landed in a weird data exfiltration state
* YARA Downloads rules from URL -- https://github.com/osquery/osquery/issues/6558
  - Does this alleviate the data exfiltration question? (seph thinks
    not, it's the same risk profile)
  - There is interest in both
  - Discuss in issue
* Extended Attributes -- https://github.com/osquery/osquery/pull/6195
  - This has been rebased. Can folks take a look?
* Systemd PRs
  - These depend on dbus, which brings in a different xml parsing
    library than we already use. Either we patch the upstream, or have
    two.
  - The PRs will need some work to bring the deps into
    cmake. Otherwise, we risk bringing in system libraries

### aarch 64

https://github.com/osquery/osquery/pull/6336

Last time we thought this could be merged and in an unsupported
stage. We still think that.

## Windows 32-bit support

https://github.com/osquery/osquery/pull/6543

> Breakwell's most recent note
> https://github.com/osquery/osquery/pull/6543 is interesting. I'd
> like to know if there are really no other ways to enforce the
> truncation errors.

Zach took a look at this. Enabling it would pull in other things we
don't want as errors. Can we run the build with this option, and see
how many errors we get? Stefano says that yes, we'd have to replace
the implicit casts with explicit casts. It's doable but cumbersome.

Discussion about whether it's possible to collect the compiler output
and parse the the warning we don't want. While possible, it's kinda
ugly. Let's say it's an option.

clang might support erroring on this, using `-Wconversion`, which
might help in scoping. Though running the windows build does seem
easiest.

## Agenda down here

## 'Lots of SST Files' problem

> Teddy is doing some analysis on RocksDB storage over time, trying to
> simulate the "lots of SST files" situations. It would help if anyone
> has sample databases they can provide. Hopefully I can find a way to
> stop this from happening.

https://github.com/osquery/osquery/issues/4333
https://github.com/osquery/osquery/issues/5425

(Discussion here blurred a bit with the next item)

## Is there interest in a max db size? 

> Potential discussion topic related to osquery's storage
> expectations: should we add a "max osquery.db size"? If we did, say
> 20GB default, what would we expect to happen if osquery hit that
> limit?

Some discussion points:

* would this apply to the pending SSTs, or to the entire on disk
  footprint?
* Is this limit to limit the disk utilization? Or to limit some kind
  of performance impact?
* Does the watchdog make this worse? The rocks DB stuff _should_ be
  outside the watchdog, but is probably not.
* We have existing flags for how logs are buffered, are they
  sufficient?
* There's existing conversation about how we misuse rocksdb (around
  transactions and indexes), if we fix that, does this problem go
  away?
* There are rocksdb options about how many SSTs rocksdb has. Do those
  apply?
* On an endpoint there is already an implicit disk limit (disk is only
  so big). So what happens?
* Even if we do the rocksdb refactors, it is possible to have a case
  where the DB can't keep up with the writes. So how do we handle that
  case.

## Upcoming refactoring

> Teddy is planning on more refactor work, moving the ephemeral
> database plugin into the 'default' plugin, and adding clearer APIs
> for initializing the database. This should not change anything
> except for making test boilerplate code simpler. After that he plans
> to add some mock targets to further simplify testing.

## Look at old PRs 

_(If there's time, we've been trying to re-visit old PRs)_

[Reverse Sorted List of PRs](https://github.com/osquery/osquery/pulls?q=is%3Apr+is%3Aopen+sort%3Acreated-asc)
