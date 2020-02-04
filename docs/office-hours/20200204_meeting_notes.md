# osquery office hours 2019-02-04

YouTube Link:

## Announcements and Highlights since the last meeting

## Any PRs/issue people want to discuss

### Diff Epoch and Counter

https://github.com/osquery/osquery/pull/6223

For the scheduled diff queries, `counter` is the number of _scheduled_
executions. When re-constructing diffs, this is not as useful as
having the number of results generated. This PR moves the counter
increment to _after_ the diff check.

Discussion in office hours that convievably someone might be using
this to track scheduled executions. But as that data is available in
`osquery_schedule` it seems odd to track it here too.

Seshu (from Uptycs) says they only care about 0 vs not-0, they treat
this as a boolean. This PR seems fine.

### Implement community_id_v1 hash function in the SQLite database

Useful for correlating events across different software such as
Zeek/Suricata/etc.

PR Link: https://github.com/osquery/osquery/pull/6211

External links:
 - [Community ID support in Zeek/Bro](https://blog.zeek.org/2019/07/an-update-on-community-id.html)
 - [Official Community ID specifications](https://github.com/corelight/community-id-spec)

This is implemented as a function, and not a table column, to allow
end users to decide where they need it. How it's used is going to be
very site dependent.

### Implement container access from tables on Linux

Implements a reusable framework for joining mount namespaces, and
integrates it with two tables. Useful for implementing container
introspection, regardless of the containerization technology used
underneath.

PR link: https://github.com/osquery/osquery/pull/6209
Blueprint link: https://github.com/osquery/osquery/issues/5975

Additional notes: the PR is a little big, could use more reviewers

## 4.2.0 (Security Release: TLS Cert checking)

osquery does not correctly verify TLS hostnames on the logging
endpoint (https://github.com/osquery/osquery/issues/6212). It will
connect to _any_ valid server cert. This bugs comes out of the 2.10
release refactoring to use beast. This can be mitagated if you provide
an expected server cert, though many use cases use the underlying
system trust stores.

After some discussion on slack, we think it's worth cutting
[4.2.0](https://github.com/osquery/osquery/milestone/44) a little
early for this. All outstanding issues have been moved to
[4.2.1](https://github.com/osquery/osquery/milestone/48), and the
remaining ones are TLS related.

Needed code fixes:
1. https://github.com/osquery/osquery/pull/6044
2. https://github.com/osquery/osquery/pull/6197

Huge shoutout to timb on slack who found this

## BPF integration

Starting small, integrating the
[ebpfpub](https://github.com/trailofbits/ebpfpub) library. Initially
focusing on adding useful events (ideas: dns events, load/unload of
kernel modules, setuid/setgid, etc...)

Roadmap:
1. **readline_events**, capturing user interaction with bash (see
   [Blueprint #6226](https://github.com/osquery/osquery/issues/6226),
   and [demo](https://asciinema.org/a/8Ud1Vm5MtfvxRRZUkVMGmuW65)).
2. **dns_events**, capturing **gethostbyname()** (and variants)

## Any comments about the osquery@scale conference?

Some conversation about how the conference went.

* Maybe about 120 people
* Lots of interest in osquery
* Lots of Uptycs focus, including some neat things they've been doing
  on their branch. General sentiment that we'd love to see Uptycs PRs
  make into OSS, but it sounds like there's a lot of divergence. We
  might see some PRs coming in from them.
* Zach gave a talk -- `osquery performance at scale`. Slides --
  https://www.dactiv.llc/blog/osquery-performance-at-scale/
* Videos coming in the next couple of weeks

## Making osquery more accessible

Zach put together a Docker container to spin up an osquery / fleet /
ELK. This is designed to be both an intro to the osquery stack.

If you're interested, check out
https://github.com/dactivllc/osquery-in-a-box

Some related things people might integrate:
* groob has an extension to pretty print logs
* seph (kolide) push launcher docker containers with fake serial numbers suitable for
  this kind of work
