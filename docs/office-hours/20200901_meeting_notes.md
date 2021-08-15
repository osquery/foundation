# osquery office hours 2020-09-01

YouTube Link: https://www.youtube.com/watch?v=7vWV1z5LTvs

## Announcements and Highlights since the last meeting

## Any Questions / Issues / PRs people want to discuss?

The intent of this section is to provide a clear time for community
members to bring up _anything_.

Broad questions? Bugs? Deployment questions? Blocked PRs?

### Adding sigurl to get YARA signatures

_https://github.com/osquery/osquery/pull/6607_

Discussion of why the url should be supplied in the config file and
not in the query, or vice versa.  If multiple people can edit the
osquery config, then it should be in the config?  If the config and
the queries have equal access privilege then it is an unnecessary
redirection?  The resource changes often but the URL is relatively
static.  Seph favors the URL in the query and the config restricting
the URLs (domains?) allowable

The goal of the config file is because yara rules are loaded into ram,
so folks want some degree of limitation on it.

seph is also unsure what this adds over joining against the curl
table's results. Even if true, this may not be a good reason not
to. It's a lot simpler to query by URL.

Some discussion about whether whether this can be added to the
_existing_ [yara
signatures](https://osquery.readthedocs.io/en/stable/deployment/yara/)
block in the configuration. We could detect the protocol. seph does
not think this alliviates his usage comments.

## Release 4.5.0 ?

_https://github.com/osquery/osquery/milestone/50_

We've got some good fixes. Especially the rocksdb stuff, we're now
forcing compaction on startup and every hour thereafter.

The big ticket items are progressing, and not worth holding stuff.

Mike would like #6607 to land in this. So maybe let's wait a couple
days for this, but not weeks.

## ARM64

https://github.com/osquery/osquery/pull/6612

A new contributor, Benjamin, has pushed this a bunch forward. The
biggest blocker now is to update the commits to include attribution to
the original author on these.

### Offshoot conversation about managing library dependencies

A heads up that this adds _additional_ complexity to the already
complex process of updating libraries. We need to maintain separate
libraries and artifacts for the builds. This will increase the burden
of updating libraries to people who have access to this hardware.

We talked some about theoretical ways to manage dependencies. No clear
conclusions

We probably need documention on how to introduce new libaries. And a
small amount of tooling to help spin up VMs and do this.

## Windows 32-bit

https://github.com/osquery/osquery/pull/6543

We think this is mergable as is. There are still some warnings about
"possible loss of data", but we can merge as is.

Current theory is that the best way to enforce that in CI, is to have
some hackiness that greps the CI output for errors.

## Windows Registry Tables (seph)

How do we feel about tables that are convinience wrappers around the
windows registry? seph is unsure...

Zach sees some value, especially if they doing additional work

Maybe we need some kind of guideline about it?

An discussion in favor is that osquery's job is to abstract this for
people. Making these useful is a huge value add.

Some discussion about different models people have for using
osquery. No clear consensus.
* seph thinks about osquery as an api translator. It makes
  inaccessible things accessible
* Teddy thinks about it as presenting valuable information ready to go
* Some of this could be presented through packs and queries.

This can also be seen as a discussion about what level of of
abstraction osquery should adhere to. Including whether or not we
define one at all.

Presenting data ready-to-go can come with a couple of caveats:
* It can be incomplete
* As OS apis and paths change, it creates ongoing maintenance work
* It can omit key information (for example, macOS MDM settings are not
  returned in most of the osquery tables)

Another way to see this, is that some tables are low level API
abstractions (plist, registry, etc) while others are helpers on top of
those (background_activities_moderator).

## Look at old PRs

_(If there's time, we've been trying to re-visit old PRs)_

[Reverse Sorted List of PRs](https://github.com/osquery/osquery/pulls?q=is%3Apr+is%3Aopen+sort%3Acreated-asc)
