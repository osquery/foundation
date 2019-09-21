# osquery office hours 2019-09-17

[YouTube Recording](https://www.youtube.com/watch?v=GStkartir-Q)

## Highlights since the last meeting

* 4.0.1 Shipped (and 4.0.2)

## Linux Bug Postmortem (this triggered 4.0.2)

https://github.com/osquery/osquery/issues/5793

Two issues in the third-party dependency bugs:
- RocksDB was missing a patch that was lost during the refactor (it
  was built into the brew-distributed binaries).
- The AWS library was missing the OpenSSL support, which would cause
  the S3 signature classes to crash.

At least two bugs on the package:
- The owner was set incorrectly. It should have been `root/root`, but
  was instead the build user.
- The systemd unit script was incorrect. Previously, it had been
  pre-processed in the old `make package` target of oldmaster. This
  was not ported, and was thus broken.

There's been discussion online and in this meeting, about how we need
more testing to catch this kind of error. We used to have a set of
python "blackbox" tests. They would iterate though the command line
arguments, etc. Stefano is looking at re-enabling them.

### What's missing from 4, that was in 3?

AlexM askes what, if anything, was missing from version 4.

- Blackbox testing
- Building extensions

## Third Party Dependencies

There are a couple of third party dependency things happening right now.

The current `third-party` folder contains the previous Buck libraries,
and should be moved inside the `libraries` folder.

Upgrading third party dependencies, and migrating them to towards
`source`.

Does moving dependencies to `source` also handle the [glib2.12
support](https://github.com/osquery/osquery/issues/5745). Not
directly, though the work may be batched.

## Reproducible builds

If we link statically, we lose the ability to use address space
layout randomization (due to lack of static PIE).
Deciding what we want here will be a future blueprint.

## Planning 4.1.0

Theme: packaging and extensions

https://github.com/osquery/osquery/milestone/43

Can we cut something sooner for Zach's work on sqlite. Releases are
still a lot of toil. This might look like a 4.0.3. Depends a bit on
what Facebook thinks. (Since they're the ones cutting releases right
now).

Probably low priority: some flaky tests were found on either Ubuntu
18.04 or CentOS 7. These failing tests are not probably caused by
anything wrong in the tables, except for the eBPF ones (that are
however testing experimental features that are probably not much
used).

- https://github.com/osquery/osquery/issues/5804
- https://github.com/osquery/osquery/issues/5805
- https://github.com/osquery/osquery/issues/5807
- https://github.com/osquery/osquery/issues/5806

## Release Planning

Some discussion about what we'd like release cadence to be.

We want to do releases for major features. We also want to be able to
ship tables and bug fixes from people. So whatever we do needs to
cover those cases.

We'd like a high release cadence, but there's a lot of toil in
creating releases. We need to get the toil down before we can ship
more regularly.

A couple of people think it would be great to have roughly monthly
releases to handle bug fixes. But we need to get the toil down first.

Alessandro started playing with a nightly build
machine. https://osquery.alessandrogar.io/packages/

### What do we want to ship anyhow

There's some people that want plain binaries. We should enable tarball
output.

Clearly some other people use packages. We see a lot of slack
requests. Especially for the MSI.

Maybe some people want to run their own builds? If there's an easy
way, maybe more people would build it themselves. Especially if builds
were reproducible and verifiable. Counterpoint, is that not everyone
who packages, is going to want to create a multiple platform build
environment.

seph thinks we want to ship:
- source code
- binaries
- packages, maybe

Discussion about whether a package should ship a config file, and
start a daemon. seph thinks it's a horrible idea -- too much variation
across sites. Sharvil thinks it might help users a lot, if we did ship
something or a better guide. This might look like making a good
QUICKSTART guide for small-medium deployments. This sort of guide
doesn't need to solve people's problems, but it should help call out
what those problems are.

Shipping packages is a batteries included approach.

## Packaging Tooling Discussion

### CPack

Initial refactor proposal (Alessandro)
- Make the CPack packaging support standalone. This means it will have
  to be enabled explicitly at configure time.
- Move all the `install()` directives in the main CMake, to preserve
  the simple `make install` command from the build directory.
- Enable tarball support

What does standalone mean?
- The `install` target moves to the main cmake files (honors
  DESTDIR/PREFIX)
- The CPack configs would become separate. This enables CPack to be
  run standalone or as part of the Makefile

Does CPack support what we want? It's a monolithic c++ program, so we
get what it does. But, it mostly calls system tools, and it offers
various hooks to those.

CPack isn't a great tool for the folks that understand the native
packaging tools. It's a pretty big wrapper, and it's hard to
understand. This isn't a blocking objection, ultimately someone owns
the code, so someone will need to be happy about it.

If we simplify what we want from the packages -- put some binaries on
disk -- then there are fewer people that are likely to want to look at
and patch the packaging process.

## Github Permissions & Billing Plan

https://github.com/osquery/foundation/issues/19

We want to give people the ability to help us. (Mike Myers @ ToB is
the catalyst this time). It seems like the most correct way here is to
use the GitHub _triage_ role. But, this doesn't seem to be present in
our GitHub organization.

seph and Teddy have both reached out to github about changing our
plan, but we have not yet heard back.

We think that while we work through the github side, we should grant
Mike committer access. There are no verbal objections in the
meeting. seph, groob, and Zach are all available for private slack
messaging if people have objections they want to bring up privately.

## False positives in macOS attack Keyboard_Event_Taps query

https://github.com/osquery/osquery/issues/5811

### Shipping packs more generally

There's some discussion about how we ship packs that have
dis-recommended queries. They're often inefficient. For example, the
browser extensions don't use CROSS JOIN.

Should we ship these at all? By shipping them, we endorse them and
encourage their use. But, like the configuration discussion, we expect
people to monitor these and adapt to local needs.

It seems like it would be nice to _remove_ them from the general
osquery repo, and put them
elsewhere. [queryhub](https://github.com/osquery/queryhub),
[queryexchange](https://community.carbonblack.com/t5/Query-Exchange/idb-p/query_exchange?utm_source=social&utm_medium=social&utm_campaign=none&utm_term=none&utm_content=site),
the docs, [rhq](https://rhq.reconinfosec.com). Of course none of these
are real enough to serve this purpose yet.

If we did make this change, it would be breaking. So we should announce it via the blog.
