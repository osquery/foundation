# osquery office hours 2020-12-08

YouTube Link: https://youtu.be/ATHtJ36LmAA

## Announcements and Highlights since the last meeting

## Any Questions / Issues / PRs people want to discuss?

## Abstract discussion about how to help people understand what osquery is


Some discussion about how to handle the broader question of "What is osquery?" and "What do I do with this?" Specially as it applies to osquery being only a part of the ecosystem. (eg: osquery doesn't have a fleet manager or log aggregation together)

We need a good archtectural diagram of this ecosystem.

Do we understand what things people see? What parts of the docs are used? Looking at the analytics on readthedocs, `index.html` is main one. With expected hits to configuration and deployment docs.

Comments about a lot of the docs/blogs/community articles are about how to handle setup. But very little about how to use it. What do you do with it once it's all running. 

## Community Involvement

Inspired a bit by "zeekdays"... Could we try to encourage people to submit queries by holding some short lived thing, and then award some prizes based on "Best query for SRE" or whatnot. Some other OSS projects have done this. 

osquery does seem to show up in some CTF things. Can we reach them? What about a CTF/hunt entirely inside osquery? Ru queries, to find the flags hidden. Akin to https://osquery.live/ or the golang tour. 

## Apple Silicon Machines (M1)

Since this all hit general availability Kolide has started to see some rosetta2 crashes. These Seem to be in the go side of our agent code, not in osquery. Anyone else seeing anything?


## 4.6.0 Release

We'd mostly decided an early december release. How are people feeling?

seph has moved most of the untouched issues from 4.6.0 to a new 4.7.0.

Suggested Holds:

* The Windows `users` table work Mike is doing -- [PR #6782](https://github.com/osquery/osquery/pull/6782)

The eBPF tables are on master, and will be in 4.6.0. There is a bug in the linux kernel around how `execveat` is handled. So the new tables may have some bugs



## Look at old PRs 

_(If there's time, we've been trying to re-visit old PRs)_

[Reverse Sorted List of PRs](https://github.com/osquery/osquery/pulls?q=is%3Apr+is%3Aopen+sort%3Acreated-asc)
