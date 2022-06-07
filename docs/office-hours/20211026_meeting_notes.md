# osquery office hours 2021-10-26

YouTube Link: https://youtu.be/i9jJdR8yeho

## Announcements and Highlights since the last meeting

* Apple released macOS 12 Monterey. No new known issues, tests pass.
* M1 support is ongoing, starting to shape up to a binary (Thanks ToB)

## Any Questions / Issues / PRs people want to discuss?

### Running osquery as a non-root user

osquery will work. but some data may not populate, depends on permissions granted at runtime.

There's been some historic community work around playing with linux capibilities, and other hardening. Start by digging into https://github.com/osquery/osquery/issues/6484 and related issues. 

### Can you use cgroups for resource constraints and disable watchdog?

While cgroup will correctly enforce constraints, there are some caveats:
1. Watchdog log and denylist misbehaving queries. This may aid debugging
2. Watchdog _may_ be a more database friendly way to enforce limit

### Password Policies set on endpoints

What do people know about password policies?

Windows -- in the registry?

macOS:
* there's an account policy table, but it's imperfect
* There's profiles. Sharvil thinks there _might_ be an API.
* There used to be an API. (https://developer.apple.com/documentation/opendirectory/odrecord/1470047-passwordpolicyandreturnerror?language=objc) but it got deprecated in macOS 10.9, and I will have to research if there are alternatives, but I don't know off the top of my head - Sharvil

linux -- pam, and a couple of other places.

Relatedly, is that if these policies are set, you also need to check that a current account has a password that _postdates_ the policy creation. 


## `output_size` column in `osquery_schedule` table

[issue 7147](https://github.com/osquery/osquery/issues/7147)

* Any context on why exactly this was removed (this was during the code lockdown it seems and there is no PR attached)
* Apparently it seems that it was inherently not a reliable metric? Would there still be value in having this, even if the data was "inaccurate" (I haven't really dug into this yet)
* If we are not going to reintroduce this column back, perhaps we should remove it from the spec and the query pack -- it seems to be causing some confusion

This has come up recently in office hours. Consensus then, and still now, is that data here would be valuable. Even if slightly inaccurate. 

## CA file updates

https://github.com/osquery/osquery/pull/7364

> I'm requesting the osquery technical committee verify the integrity of certs.pem. In addition, should we add a release note to check if Mozilla CA certificate stores have been updated as to ensure ongoing maintenance of certs.pem?

seph's feeling is that we should be willing to accepting tooling to download, but that we should be more cautious in accepting new bundles. 


## puffycid PRs

TSC has plans for how we can move forward with some of these:
 - https://github.com/osquery/osquery/pull/7258
 - https://github.com/osquery/osquery/pull/7259
 - https://github.com/osquery/osquery/pull/7160

TSC will evaluate:
 - https://github.com/osquery/osquery/pull/7261 - Add Amcache and Raw Registry Table
 - https://github.com/osquery/osquery/pull/7260 - Add Jumplist table

Discussion about:
* what PRs we think osquery thinks we want to merge, vs what things we are highly unlikely to merge.
* whether osquery is a good tool for forensics, and whether or not we _want_ to develop towards that.
* Zach says that in his consulting history, there are sites that do use osquery for forensics. 
* Extensions as tools that have more options and sandboxing support
* Revisiting the idea of an official osquery forensics extension
* general attack surface of libraries and parsing code
* But many of these concerns don't apply to these specific PRs

## Look at old PRs 

_(If there's time, we've been trying to re-visit old PRs)_

[Reverse Sorted List of PRs](https://github.com/osquery/osquery/pulls?q=is%3Apr+is%3Aopen+sort%3Acreated-asc)
