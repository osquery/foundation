# osquery office hours 2020-10-13

YouTube Link: https://www.youtube.com/watch?v=nBb9ZIXR4gQ

## Announcements and Highlights since the last meeting

### 4.5.1 was cut

Not yet flagged as stable. Builds are uploaded, folks should test.

## Any Questions / Issues / PRs people want to discuss?

The intent of this section is to provide a clear time for community
members to bring up _anything_.

Broad questions? Bugs? Deployment questions? Blocked PRs?

## Extra CI Hosts

seph has been inching along with aarch64 and apple silicon. What's
up?

We have some AWS credits, slated for use with aarch64 (yay)

We have an apple silicon dev box (yay)

What we don't have, is a good way to hook these into CI. The GithHub
docs are very clear that just running PRs is tantamount to running
untrusted code on the machines.

There's a bit of a story for doing that on AWS machines. Spin up VMs,
run a batch of tests, destroy. The envoy project does that
[here](https://github.com/envoyproxy/ci-infra). There is no story for
doing that on apple silicon.


## macOS Endpoint Security PR

(Ted, many others interested in updates)

We can assign a ToB person: Sharvil, or Mike if Sharvil can't be
assigned

Can we add non-core members as developers to the apple account?

ToB is starting to setup profiles under their account

TODO: Let's add some people as needed

### CI Machines

Shavil has been moving CI stuff to catalina, and is prepping new xcode
versions for the upcoming Azure deprecation

## Extensions

### Builds

We seem to have a persistent stream of questions about building
extensions. [#6702](https://github.com/osquery/osquery/issues/6702) is
the most recent. (this is probably a cmake failue. Alessandro will
answer)

The way the extensions are build is very weird. It requires highly
unusual symlinks or cmake processing. This is partly because:
* Reusing helpers
* code that defines extension vs plugin
* Extensions that have not been built with the osquery-toolchain will
  not be portable, and will only run on the same Linux distro used to
  build the binaries. Additional issue: the binary can easily break if
  the libraries on disk are updated by the packaging tool of the
  distro. This is fine if the developers are willing to invest time on
  their own on a toolchain + redistributable dependencies
* There might be history around wanting to make it easy to move code
  from an extension to core.
* There was effort to make it easy to ensure the osquery and sdk
  versions are in sync. Everything just builds together.

Does it make sense to ship a clean room SDK based on the thrift
interface? We could ship helpers or not. This would probably be easier
for people that don't ever push code to core, but would be harder to
share code.

Have we ever moved code from an extention to core? No one can remember
it.

If we ship a bare sdk, it may make it _harder_ for people to link
things.

It sounds like the conclusion is that there's general interest in
shipping a bare SDK, but it's not clear that people want to do that
work. But it's an option for the future.

### Writable Extensions

Something might be broken. no bug yet. Reportedly, the `WHERE` clause
can no longer be used with `DELETE`/`UPDATE`

The registry would issue special actions via Thrift whenever UPDATE,
DELETE and INSERT statements were used. Recently, something broke, and
those statements are now issuing a `generate` action whenever a WHERE
clause is used.

Issue: https://github.com/osquery/osquery/issues/6710

## Planning for accepting new contributors

We'd like to grant more people commit access for merging PRs. What
should our criteria for that be? Some brainstorming
* Track record of accepted PRs?
* Track record of reviews?
* How can we keep a low key nomination process.
  - Support for self nomination
  - Should be something that feels right?
* discussion about how people feel around identies. Hesiticy to add someone unknown to us.
* bootcamps, a bit to get people involved, and to set expecations
* A potential process:
  - Someone is nominated (by themself is okay)
  - slack or github
  - dicussion happens 
  - unanamous? Majority?
  - done

Right now, not many people actually click merge. (Mostly seph and
Teddy). So what we really need are people who can review
code. Counter-point -- people can feel more investment if they have
commit access.

Ideally people should be able to re-start their own CI.

Some discussion and concerns about what permissions translate. GitHub
is pretty monolithic, so what risks do we have.

## Look at old PRs 

_(If there's time, we've been trying to re-visit old PRs)_

[Reverse Sorted List of PRs](https://github.com/osquery/osquery/pulls?q=is%3Apr+is%3Aopen+sort%3Acreated-asc)
