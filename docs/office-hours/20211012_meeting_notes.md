# osquery office hours 2021-10-12

YouTube Link: https://youtu.be/gF1IEnrZYXw

## Announcements and Highlights since the last meeting

## Any Questions / Issues / PRs people want to discuss?

* PuffyCid reminds us he has several PRs waiting
* Discussion about whether new tables embiggen attack surface, or incur maintaince burden
  - No clear answers
  - Not even clear what the criteria should be

## New Windows and macOS

Various people have osquery running on Windows 11 and macOS 12 Monterey. No exhaustive testing, but no issues seen to date. 

## Vague reports of windows upgrade issues

Some discsussion that some `orbit` users saw windows machines go offline during an update. No real info, not sure if this is real. 

## Structuring the PR for Apple Silicon library support

So, will we have to break up this PR? How to merge things before CI exists for it?

The crowd seems amenable to either: a big PR or lots of small ones. 

## Apple Silicon CI

Some discussion of ephemerality for CI and Build machines. It's not yet clear if we have good options for Apple Silicon. 

Ephemerality most important for a release, could compromise for CI. 

What about cross compilation for the build, and testing would be non-ephemeral? This might work? There's a bit of TBD. Some discussion of this in https://github.com/osquery/osquery/issues/7325

https://www.macstadium.com/orka maybe?

Not clear if https://www.macstadium.com/vmware is enough either?

## How do we feel about NTLM hash exposure

seph is discussing whether it's reasonable to expose an NTLM hash. The storage schema on the SAM hive is a weird obscufiated system. But there's code out that to reverse it. Some considerations:
* probably okay privacy wise
* Feels iffy security wise, these live forever, and we don't do it for `shadow`
* For something like checking if the password is blank, we chould expose a boolean for it
* How would one query for password reuse?

## Discussion about osquery views

A tangential discussion about whether we should support more tables that are simple wrappers over the registry. 

Maybe we could ship views? It would be neat to see more use of views. 

Alessandro talks about views being interesting in query packs, but is concerned about name collisions.

Is there a time someone would want to shop a view to scheduled queries, but not distributed queries? (No one has an example)

## Look at old PRs 

_(If there's time, we've been trying to re-visit old PRs)_

[Reverse Sorted List of PRs](https://github.com/osquery/osquery/pulls?q=is%3Apr+is%3Aopen+sort%3Acreated-asc)
