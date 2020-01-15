# osquery office hours 2019-01-07

YouTube Link: https://www.youtube.com/watch?v=evSdjwriOhk

## Announcements and Highlights since the last meeting

### Teddy Approaching Parental Leave

Cutting new releases will hard. But we think we'll be okay. Super
excited for Teddy.

TODO item: Push 4.1.2 to stable in the linux repos, upload to chocolaty

## Any PRs people want to discuss

The intent of this section, is to allow contributors to bring up and
PRs they feel blocked on.

* [PR#6140](https://github.com/osquery/osquery/pull/6140):
  `chrome_extension_content_scripts` table
* [PR#6153](https://github.com/osquery/osquery/pull/6153): Environment
  expansion utilities, improvements to tables for Windows

Folks will try to review

## Tests in Buck

We've had some issues around adding tests in Buck, relating to include
path lengths. This has come up in past office hours.

The real for for this requires restructuring a bunch. Teddy had
started some work for this, but we're not sure what the state is.

We think we shouldn't block future work on this. So we should merge
what we can, and the buck tests may diverge from cmake until we
resolve this.

There's dicussion about how we could/should be using buck.

## CLI Flag Semantics

In [pr5882](https://github.com/osquery/osquery/pull/5882) we have
disussion about how today, there is no functional difference between
`FLAG` and `CLI_FLAG`. This is clearly a bug we should fix.

What should happen with the things marked `CLI_FLAG`? Today, the code
implies you cannot set them via the config, but we hear that people
are doing so. So we should settle on what our _intent_ with cli-only
flags is, and then enforce it.

Two theories have arisen:
1. `CLI_FLAG` is for things that _must_ be configured to bootstrap a
   remote server. This was the original intent, though this may not
   make sense any more.
2. Privacy/performance concerns. For example the carver function. Is
   this something we want an endpoint to make an explicit choice to
   enable?

Discussion about whether it makes sense to have `CLI_FLAG` flags?

There seems to be consensus that this is a very site specific, and
relates to the site's security posture and feeling.

There's general consensus to keep both options. But much less
consensus whether anything is `CLI_FLAG` only. (ignoring things like
interactive mode)

This can be followed up in the
[pr5882](https://github.com/osquery/osquery/pull/5882)

## Implement `Enable Tables` Flag

https://github.com/osquery/osquery/pull/6150 is an (unfinished) PR to
implement a whitelist for tables. This is the opposite of the
blacklist created by `disable_tables`. Do we want this functionality?

General consensus is that the functionality seems reasonable. It's an
obvious fit with the `disable_tables` functionality.

Discussion about precedence? Blacklists show up sometimes around
stability, especially for version skew. Probably blacklist would take
precedence. Whitelist defaults to `*`, blacklist defaults to `nil`

Discussion about how this might merit a new section on the docs about
enabling and disabling tables.

This can be followed up in the PR

## Upgrading OpenSSL version

The current library version has reached EOL. In order to update it, we
have to move everything to the source layer. Any work started on this
yet? How do we tell what's left?

1. Look at CMakeLists.txt's library_descriptor_list
2. Match those against libraries/cmake/{source,source_migration}/modules


Can be tested by removing the "facebook" layer from the settings:
https://github.com/osquery/osquery/blob/c18f5bc75c84ec1891309b5dee07a0d459997a83/cmake/options.cmake#L83

```
if(DEFINED PLATFORM_LINUX)
  set(third_party_source_list "source;formula;facebook")
else()
  set(third_party_source_list "source_migration;facebook")
endif()
```

Important: Some of the packages in the facebook layer are still
required at build time for the codegen Python scripts. It is alright
to import the following libs:

* jinja2
* markupsafe

An incomplete list of the libs to migrate:
* popt
* augeas
* llpd
* sleuthkit
* smartmontools

There is potentially an API change here as well, so we'll need to be
cognizant of that.

## Arch Linux

There's been some tinkering around getting into Arch linux.

Two issues...
1. We need a layer to pull in OS dependancies.
2. Arch doesn't have libc++ in the way we need it


## Caching built libraries, especially on Windows

Builds are way too long, we really need some kind of build
caching. The developer experience is bad.

Stefano looked at some of the existing ccache equivlents for
windows. He did not find a working open source one.

When we build libraries in the build folder, the layer is not the same
as installing a standalone library. This makes it hard for a developer
to use an existing library install as the dependecy. This would also
provide a place to shim a cmake dependancy layer.

Right now, can we just zip the folders up? This might not be possible
because the headers are in an shared location. Discussion about
strategies for how to support this.

One thing we need is a way to host/serve binaries for a pre-compiled
layer. ([@directionless](seph) will take this as a todo)


## Thoughts on current BPF implementation

* Currently supports only setuid(), kill() system calls
* Is anyone using them?
* Seems to be (partially?) depending on the unfinished event framework overhaul
* If we reimplement them, in which table do we present them?

There are a handful of interesting ideas here:

1. Create something like ATC. Allowing config time creation of BPF programs
2. Replacing auditd based tables. Potentially better performance
3. Implementing new tables

There are some issues with replacing auditd tables. For example, older
kernel compatibility.

Creating runtime tables may be tricky as well. Doing rich things
probably requires making it as a table.

Do we know if anyone is using the existing BPF functionality? It
doesn't look like anything in osquery core is using ebpf. It looks
like it merged in https://github.com/osquery/osquery/pull/5354, it's
not clear that this is used anywhere. Maybe internally at Facebook?

Alessandro will prepare a PR to implement a `system_call_events`
table, so it can be discussed on Slack/office hours. System calls that
could be interesting to trace: setuid, kill, kernel module loading,
etc.


















## CLI Flag Semantics

In [pr5882](https://github.com/osquery/osquery/pull/5882) we have
disussion about how today, there is no functional difference between
`FLAG` and `CLI_FLAG`. This is clearly a bug we should fix.

What should happen with the things marked `CLI_FLAG`? Today, the code
implies you cannot set them via the config, but we hear that people
are doing so. So we should settle on what our _intent_ with cli-only
flags is, and then enforce it.

Two theories have arisen:
1. `CLI_FLAG` is for things that _must_ be configured to bootstrap a
   remote server. This was the original intent, though this may not
   make sense any more.
2. Privacy/performance concerns. For example the carver function. Is
   this something we want an endpoint to make an explicit choice to
   enable?

## Implement Enable Tables

https://github.com/osquery/osquery/pull/6150 is an (unfinished) PR to
implement a whitelist for tables. This is the opposite of the
blacklist created by `disable_tables`. Do we want this functionality?
