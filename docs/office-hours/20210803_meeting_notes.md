# osquery office hours 2021-08-03

YouTube Link: https://youtu.be/eWMKI9iN_WA

## Announcements and Highlights since the last meeting

Some upcoming talks!

* DEFCON/BTV: workshop on threat hunting using the Osquery agent -- Ben B
    * MacOs Workshop - Hunt for Red Apples: Ocean Lotus Edition
    * https://dc29.blueteamvillage.org/call-for-content-2021/talk/XXLJLJ/
* DEFCON/BTV: Yeet the leet with Osquery -- Sebastiaan 
    * https://dc29.blueteamvillage.org/call-for-content-2021/talk/VXEQKN/
* BSidesAugusta: Automatic prefiltering for Osquery: WHERE do I sign up? -- Josh Brower
    * https://bsidesaugusta2021.busyconf.com/activities/60f09155cf2efa007fed7af0


## Any Questions / Issues / PRs people want to discuss?

The intent of this section is to provide a clear time for community members to bring up _anything_.

Broad questions? Bugs? Deployment questions? Blocked PRs?

### PR 7216: Fix AWS tables when using IMDSv2

https://github.com/osquery/osquery/pull/7216 should be easily approvable now, requested changes are done

### PR 6904 macOS unified logs

https://github.com/osquery/osquery/pull/6904


## Should the Install Paths Be Configurable?

Some discussion on about whether or not the install paths should be configurable. This was brought up by one of the community members as part of a anti-malware action.

seph is not a fan, but thinks that if it's easy to maintain in the packaging scripts, it's low cost. Though notes that there are potential code signing issues. 

Stefano thinks there may be places in the code that check on the name. Maybe the pid file?

An alterante route here is that one can not use the osquery packaging. osquery is a binary, you can move it around and do your own service management. 


## 5.0 Release Milestone

osquery 4.9 was June 14th, so it has been ~6 weeks when we were planning 5.0 to be a minimal diff from 4.9 of just breaking changes. Let's get this unblocked -- MM

https://github.com/osquery/osquery/issues?q=is%3Aopen+is%3Aissue+milestone%3A5.0.0 can we go down these one-by-one and commit to completion this week? Suggest that we start with a signed/notarized pre-release package of the macOS build so that EndpointSecurity can get some pre-release testing. 

This looks like all that's really left is the 5.0 build stuff. 
* https://github.com/osquery/osquery/pull/7184
* https://github.com/osquery/osquery/pull/7210

## Apple Silicon support

https://github.com/osquery/osquery/milestone/52 

Any objections to me filing separate issues for each of the dependencies that need to be ported for supporting Apple Silicon native builds? Or would we prefer a checklist issue. General support for making tickets

As a side note, we'll (eventually) need CI. Which brings in a lot of questions about github actions. And a question abouy where we can even host m1 machines. Though we think we can defer this a little.

We might be able to cross compile. Stefano says he make a simple proof of concept that had cmake build univeral binaries. Though if we needed different library versions for different architectures, which would be an issue. 

## Mutual TLS via the system cert stores

osquery uses a cert on disk for mutual TLS. Have people looked at using an OS certificate?

seph thinks he saw a ticket, but no real work.

Teddy thinks there may be a lot of work, if the underlying OS apis require the entire session use them.

There is keychain on macOS, and certificate store on windows. (Look at the certificates table). There probably isn't anything standard on linux, but but there might be things like gnome keychain.

## sqlite

Teddy reminds us that if we find things we want sqlite to do, we could float these with the consortium. 

Some discussion about how `QueryContext` comes through to the table generation functions. seph points out if often comes through as multiple calls with a single contraint. Teddy points out that it does work on the file table. We discuss the query planner. This can explored with `osqueryi --planner`

## developer experience / improvements

(The sqlite discussion disgressed a bit into developer experience)

There's general consensus that we need better helpers for fetching data from `QueryContext` and making rows from the data.

We have a common use case where we query a table for os data. Having a simpler internal API for that would likely help. 