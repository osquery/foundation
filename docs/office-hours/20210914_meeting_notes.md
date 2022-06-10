# osquery office hours 2021-09-14

YouTube Link: https://www.youtube.com/watch?v=WPRG8sdc16g

## Announcements and Highlights since the last meeting

* 5.0.1 Declared Stable!
* Huge thanks and shoutout to everyone for helping out/testing! (sharvil)
* Lots of new faces. Welcome everyone!

## Any Questions / Issues / PRs people want to discuss?

The intent of this section is to provide a clear time for community members to bring up _anything_.

Broad questions? Bugs? Deployment questions? Blocked PRs?

## Browser History (puffycid)

https://github.com/osquery/osquery/issues/7177

This is a bit of followup from a prior office hours.

These artifacts would be quite valuable to forensics. But, the current sense of the maintainers is that this is too far past the privacy line.

Making it easier to deploy extensions might be a good step. Though this requires an organization be able to deploy extensions. FleetDM's Orbit is a direction. The osquery packaging has some rough hooks for this.

How do we feel about the existing tables that get pretty close to the privacy line. seph thinks that at some future point we might remove those. 

Carving is a different direction at this data. 

What would opt-in tables be like? How would it would. 

## Binary Parsing library (puffycid)

Two implicit questions in this.

Do we want to be in the business of supporting various binary formats? Maybe.

And then, if so. Is there a language that we can trust enough, so that we can use it. And if so, would it help?

Right now, we have good coverage for older format versions, because we use the supported libraries. But bringing in library like this requires that we do the work of maintaining format versions ourselves. 

There's some skepticism about what the long term maintenance cost of this would be.

What would it be like if we had a forensic extension?
* Would this be in the osquery-foundation
* Would it have different security/library policy?
* Are there things that make sense in core vs extension?

There's a lot of good contributions here, so how can we get this kind of utility to end users. 

## eBPF issues

Reports of malformed events:

 * https://osquery.slack.com/archives/CADGD9ACF/p1630616326004700
 * https://osquery.slack.com/archives/CADGD9ACF/p1631028642022100

Reports of memory issues:

 * https://osquery.slack.com/archives/CADGD9ACF/p1631224873035600

PR list:

 * Switching buffer/string capture: https://github.com/osquery/osquery/pull/7302/files

Some kernels seem to be more likely to incur this issue. Related is that page fault handling is disabled when a probe is called. So if a probe tries to read unloaded memory, it gets nothing. One solution is to change how the buffer is captured -- instead of at the start, we can capture it at the end. 

Alessandro: I am looking in the malformed events issue (the PR linked); once the execve* probe issue is fixed, I'll also debug the memory issue

## Pidfile handling

The current Pidfile implementation is subject to race conditions; there is a new PR to address that, but will stay in draft for a while until we can better test it. - Alessandro

PR: https://github.com/osquery/osquery/pull/7304 (looking for feedback, opinions)

## Package Serving (seph)

https://github.com/osquery/foundation/issues/80

Background: The osquery package server, serves about 70TB/month. This results in monthly costs north of $5k.

seph enabled logging on cloud front, and did a tiny bit of analysis. Initial conclusion is that the vast bulk of this data is a _single_ AWS customer continually downloading packages. (Frequent node refreshes?  Ephemeral Nodes? Who knows...) As so much of our traffic is to AWS, there are cheaper solutions than CloudFront. 

Specifically, serving directly from the S3 Buckets. Bucket data transfer to in-region AWS, should be free. 

seph has started playing with various ways to redirect these very active IP addresses to direct bucket download links.

## 5.x pre-flight scripts when doing upgrades

Someone on slack pointed out that 5.x does not stop the old launchd service. We haven't done this historically, should we?

This is highlighted by the identifier and path changes. Prior, the new pkg would drop drop new things over the old ones. That's doesn't work here. 

Thoughts? Should osquery do this? Cross-platform issues? There is probably stuff to do cross platform. But it might be different per platform.

Some side conversation about what a package should do. Register the service? Start the service? What about config files? Etc.

## PuffyCid PRs

* Amcache/Raw Registry Parsing - https://github.com/osquery/osquery/pull/7261
* Jumplist - https://github.com/osquery/osquery/pull/7260
* UAL - https://github.com/osquery/osquery/pull/7259
* FSeventsd - https://github.com/osquery/osquery/pull/7258
* LIEF - https://github.com/osquery/osquery/pull/7160