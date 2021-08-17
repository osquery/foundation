# osquery office hours 2021-02-02

YouTube Link: https://youtu.be/OOhRDMuWmdY

## Announcements and Highlights since the last meeting

## Any Questions / Issues / PRs people want to discuss?

The intent of this section is to provide a clear time for community members to bring up _anything_.

Broad questions? Bugs? Deployment questions? Blocked PRs?

### Question about FIM on slack

https://osquery.slack.com/archives/C0L70NC1W/p1612151563000600

* Community member question in #FIM
* Osquery FIM is being blocked by SElinux on CentOS
* Is this the intended behaviour, if so what settings need to be modified to make it work? 

No one in office hours seems to know much about selinux. Is there a way to whitelist osquery? An issue should get filled

### Sign off from someone on some 'done' PRs

* ToB -- https://github.com/osquery/osquery/pull/6897
* ToB -- [Unified Log table](https://github.com/osquery/osquery/pull/6904)
* ToB -- [Addt. column for system_extensions table to track explicit allows](https://github.com/osquery/osquery/pull/6915)
* ToB -- [Significant improvements to chrome_extensions table](https://github.com/osquery/osquery/pull/6780)
* A couple from seph as well

seph will review, and will tag in Zach as needed. 


### Is there active development for the Apple endpoint security framework?

Yes, look for a new PR in the next few business days. Working on process events. 

Network events is a different apple entitlement, and requires being packaged as an app bundle. The request is automatically approved, but needing an app bundle is a shift for us.

Sharvil will add a entilements file as part of his PR.

## Switching Publishers without restarting

If a publisher returns "disabled by config", then the eventing framework will make it as disabled. This is likely inadvertent. Alessandro is trying to fix it

There's a design issue in core -- there's no way to tell why something was disabled. Due to failures, configuration, etc. 

https://github.com/osquery/osquery/pull/6929#issuecomment-771007716


## Reports of 4.6 becoming unresponsive

https://osquery.slack.com/archives/C08V7KTJB/p1612203837055700

Report from a user that live query eventually hangs thet `CentOS Linux release 7.9.2009` machines. Unclear if this problem is specific to 4.6, or what.

We have not seen verbose logs yet. 

Zach will try to reproduce

Is this enough to pause a 4.6 release on? Let's wait at least till Zach takes a look. 

Teddy says this sounds like https://github.com/osquery/osquery/issues/6887 (which is one of several)

We'll try to look at this on Thursday.

## Packaging and Signing Pipeline

https://github.com/osquery/osquery/pull/6921

Current status is just that it needs documentation, which Alessandro is working on

* main osquery repo -- osquery! Packaging metadata (debian control, wix xml, etc...)
* osquery-packaging -- cmake/cpack code ingests above and packages
  - This is a public repo, and can run in both the public context, and a private signging repo
* osquery-signging -- **Private** repo that pulls artifacts and package code from above, and does the signing steps. 

The idea is to try to isolate and provide some safety and code review assurances on the sensitive repos. 

Next Steps:
* Need docs
* Signging repo needs signging keys
* The packaging code is all in place
* Figure out the linux side
* s3 credentials to push?

Linux package repos (using freight) needs a bunch of local state. We're not sure if it's the metadata. or the entire repo. We'll need to check. While we can download from s3, if we need to download the entire state that's a lot of download.

## Thrift Issues on Windows

https://github.com/osquery/osquery/issues/4280

Issue about the pipe disconnecting? On windows, if anything enumerates the pipe, one side ends up thinking it's a bad client and closes this pipe. This is undesired, and will cause pipe failures.

We think this is still true, even with the thirft upgrades in 4.6

We might be able to wrap something and re-open the socket. But no one is working on this, and it's not clearly reproduced enough to file upstream. 

This looks like it was https://issues.apache.org/jira/browse/THRIFT-5078 this looks to have mnerged to 0.14.0, which isn't stable yet. 

## Look at old PRs 

_(If there's time, we've been trying to re-visit old PRs)_

[Reverse Sorted List of PRs](https://github.com/osquery/osquery/pulls?q=is%3Apr+is%3Aopen+sort%3Acreated-asc)
