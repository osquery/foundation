# osquery office hours 2021-06-22

YouTube Link: https://www.youtube.com/watch?v=AAvp4eBtrlw

## Announcements and Highlights since the last meeting

## Any Questions / Issues / PRs people want to discuss?

The intent of this section is to provide a clear time for community members to bring up _anything_.

Broad questions? Bugs? Deployment questions? Blocked PRs?

## 4.9.0

We cut 4.9.0, what's the state of packages?

We need Teddy to resign the 4.9.0 packages, since moving away from his key is the breaking change driving 5.0

## Packaging Automation

We did a bunch of iteration on the package pipeline for the 4.9.0 packages. It works now, and it's amazing. A huge amount of effort from many people. 

Google SLSA Specification -- Google has recently put out a specification around supply chain attacks. The packaging code gets us a long way there. 

## 5.0.0

Intent here is to push all the breaking changes. Still holding off on merges, until we have 4.9.0 packages.

## `read_max` parameter and `ssdeep`

It seems that `read_max` parameter (default value of 50MB) is not respected for `ssdeep` column in the hash table. It is also quite expensive, is this intentional?

`ssdeep` is a library, and the behavior of `read_max` and libraries isn't wholly clear. The other hashes do implement this, so if it's reasonable to add this why not?

## Look at old PRs 

_(If there's time, we've been trying to re-visit old PRs)_

[Reverse Sorted List of PRs](https://github.com/osquery/osquery/pulls?q=is%3Apr+is%3Aopen+sort%3Acreated-asc)
