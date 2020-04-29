# osquery office hours 2019-04-28

YouTube Link:

## Announcements and Highlights since the last meeting

* 4.3.0 packages being pushed to stable

## Any Questions / Issues / PRs people want to discuss?


## Apple Developer Cert is in flight

A couple of questions came up about the state of apple code
signing. Specifically whether any of the new options in XCode effect
us. seph thinks that this doesnt not effect us. Those options
correspond to how we invoke `codesign`, and we're using appropriate
ones to support notarization.

seph did submit our information for an apple developer cert. There are
some followup questions. Things are ongoing.


## BPF Stuff

docker and cgroups? Alessandro asks about how cgroups and docker work
together. None of us know, more research is needed.

Nishant asks why we're not adding the ppid. Is it because we're
avoiding the kernel headers? Yes, Alessandro is trying to avoid kernel
headers as much as possible, so he can keep compatibility with
4.18. There is a helper that's providing a struct, but we may not know
the location inside the struct. Some open discussion about what the
next steps here are.

Some discussion about BCC and bpftrace, using those would not provide
many advantages as they are mostly debugging tools. They usually
create large probes, and the error recovery in the base libraries is
not suitable for osquery, where we aim at running without disruption
even in case of an error.

Alessandro is in the midst of refactoring...

## TLS Certs

`*.osquery.io` was in facebook's cert management cloud, and was
expiring. As osquery is already running inside AWS, Teddy moved the
cert management into one of the AWS services.

## osquery container PRs column names

A bit of a nitpick about naming. This is meant as a fun conversation,
not a strong criticism.

The current direction adds a long column name to tables. How do we
feel about that? Does having it hidden mitgate it? Given security
software's penchant for long explcit names, this may be a non-issue.

Discussion about whether or not this can be a `HIDDEN` column. That
would keep it from confusing people who don't need it. **But** if you
run something like `select * from foo where pid_with_namespace in
(100, 200)` you can get ambigous data back. Some discussion about we
have a hook for adding `pid_with_namespace` to be visible if it's in
the where clause. This needs some future followup.

This name is a preposition. Is there another name that fits? There's
general interest in having some convention. This one was chosen
because it is very self descriptive.

Is there a way to add this column everywhere? Instead of patching each
table to handle this new constraint, is it possible to do something
more universal? Or in codegen? Stefano has tried to make it
easy. Maybe we can add a "container-ready" flag to add to tables? This
was evaluated, but instead folks decided to defer that, and work on
getting right implementations now


## Discussion about how we can test features / tables

Spawning off from the discussion about osquery container. How would we
support things like _beta_ tables or columns, that might change as we
explore their uses.

An idea about having clients declare what tables they want. This
declaration could be versioned. This could also include some amount of
table based authorization.

Lots here to discuss. No clear actions.

## aarch64

This is still hanging out on a branch. Anyone want to pick it up?

We don't have really clear CI for this. Do we want to support
something in master, that we don't have CI for?

Quickly looking around for aarch64 support:
* circle-ci: no
* azure: not directly, but there's an agent for self hosted machines
* github actions: yes

Note that performance on CI machines is highly dependant on how many
cores we get.

Most of the build system work is being done by Stefano and
Alessandro. So generally deferring to them for that decision.
