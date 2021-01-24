# osquery office hours 2020-09-29

YouTube Link: https://www.youtube.com/watch?v=Ebjkg6WpisA

## Announcements and Highlights since the last meeting

## Any Questions / Issues / PRs people want to discuss?

The intent of this section is to provide a clear time for community
members to bring up _anything_.

Broad questions? Bugs? Deployment questions? Blocked PRs?

- Ready for review, end to end tested:
  https://github.com/osquery/osquery/pull/6671 this is another step
  towards reducing the size of the build artifacts.

- The BPF PR could use a code review since it's pretty big
  (https://github.com/osquery/osquery/pull/6571). The branch is still
  missing unit tests for the bpf_socket_events table.

## Release 4.5.0

There *is* a bug in the windows on demand event log. If you specify
the wrong path for the event source, it causes a crash. See [PR
6660](https://github.com/osquery/osquery/pull/6660).

While 4.5.0 _is_ an improvement, it has a known crash. General
interest is to cut a 4.5.1. We'll discuss on slack

## Endpoint Security Framework

Table has a different name. We think this is correct.

Seperate discussion about CI -- we need to move the build to Catalina
in order to start testing Endpoint Security. Target SDK is still
10.12. There are some flaky macOS tests; we should track them in
issues.

### Can we add people to the apple account?

Some discussion about whether we can create a class of developers that
can do local signing, but not something that can be released. Mike is
in favor of this. He'll give seph an overview.

## Linux Foundation

One of the LF people is looking for folks to chat with. Who wants to
try to be in that meeting?

Several volunteers -- Alessadro, Zach, seph


## Building with third-party repos

Some of the third-party repos we rely on, appear to be unreliable
(e.g., libgcrypt). The https servers tend to go down for days. (And we
preferentially pull from https, as it offers MitM protection)

So, Alessandro has created some clones in the osquery organization
that we can sync from. This adds a step of updating these clones
before we can update a third party library.

- https://github.com/osquery/third-party-libgcrypt
- https://github.com/osquery/third-party-libgpg-error

Issue link: https://github.com/osquery/osquery/issues/6670


## Documentation Updates

The "process auditing" wiki page could use some love. It starts with a
"general troubleshooting" section, when it should start with a "how to
get started". The macOS OpenBSM now supports `--audit_allow_config` so
we can move the audit_control editting to an 'additional notes'
section. We should still document editting that file in the case that
someone wants more nuanced control over audit.

Mike will probably take a pass at this.


## Blog Posts

Teddy suggests a blog post on how to use osquery with containers. This
can document the new container-supported virtual tables and add notes
about process auditing with containers (on Linux only one process can
hold the netlink socket).

## Additional CI stuff

Some questions about alternative CI systems

1. CircleCI supports our platforms and has a hacky way to support
   aarch64. Pros for CircleCI is that you can pay for more CPU/compute
2. TravisCI has a friendly way to support our platforms and aarch64;
   cons include a strict limitation to 2 CPUs.
3. GitHub Actions

Motivations:
* The Azure account is ToB owned. This makes it harder for osquery to
  manage, and harder to think about code signing.
* Not great aarch64 support

We could use more than one system, this probably requires smoother CI
in general.

There is broad consensus for running agents on odd hardware. With
caveats around how to handle that securely, so that a rogue PR doesn't
compromise the machine.

## Look at old PRs 

_(If there's time, we've been trying to re-visit old PRs)_

[Reverse Sorted List of PRs](https://github.com/osquery/osquery/pulls?q=is%3Apr+is%3Aopen+sort%3Acreated-asc)
