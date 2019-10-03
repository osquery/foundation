# osquery office hours 2019-10-01

YouTube: https://www.youtube.com/watch?v=paVgZTBQLSg&list=PLciHOL_J7IwpSUHLTNwMZFaQAcSRSTIkI&index=10&t=0s

## Highlights since the last meeting

N/A

## Outreach

### Hacktoberfest

Last time we did this was 2017. Chatting about what we labeled as Hacktoberfest. And the intent was to get lots of general interest in the project.

Discussion is that we should be very liberal in what we label -- Mike will do this.

Then we should tweet about it. (This likely requires getting more people twitter access. See https://github.com/osquery/foundation/issues/25)

### PR Backlog

Is it time to ruthlessly close them? Several of us think it's too soon -- we haven't really done any outreach yet. Maybe we should give people a window. Respond in N weeks, or we'll either close it or take it over. 

Some discussion of whether or not we want a bot to autoclose thing. General consensus is that a bot might be too aggressive and off-putting.

TODO: Write some text we can cut and paste into PRs.

Should we spend office hours time reviewing open/active PRs? We're always willing to hear about a specific PR, if a contributor is present. It's harder to make time for all PRs.

### More C++

Teddy has often advocated that that we should have more C++ contributors. What can we do to evangelize and otherwise attract these folks? 

Are there conferences we can speak at? We think Uptycs was at BlackHat. Are there others?

Are there Slack workspaces? Some of us are active in MacAdmins, but it's more users than core contributors.

Alex mentions that ATT is still in 3.x. What, if anything, can we do to help people move. 

## Is osquery 4 seen as stable?

A disgression about whether or not people percieve osquery as a stable piece of software.

What contributes it to seeming stable?
* Regular releases
* More test coverage? Counterpoint -- Who is this visible to?

Did [ev2](https://github.com/osquery/osquery/issues/5158) merge? Maybe? https://github.com/osquery/osquery/pull/5401 happened. In any case, the [documentation](https://osquery.readthedocs.io/en/latest/development/pubsub-framework/) does not describe the new eventing framework. The original eventing framework has not been replaced yet.

As the blueprint is today, ev2 allows bypassing rocksdb. This means that that ev2 would make it more ephemeral. Is this desirable? We should have discussion on the ticket.

## GitHub Triage Support

Sharvil asks if there's an update on the triage stuff, and volunteers to do more ticket work. seph says no progress here.

## Facebook

We hear that the internal Facebook team (As lead by Ryan) has been doing a bunch of major changes. So that code is diverging more and more. 

The folks that are mostly in contact with us, work more on the open source one.

We still expect that security issues, found by fuzzing and the like will make it back to to the OSS version.

Their internal focus is on stability/reliability. We don't have clear information. If we want more info, we need to talk to Ryan.

Some discussion about moving the Windows build to Clang, which would allow testing with Clang's address sanitizer.

### Facebook and the CLA

We had an agenda note:

> Facebook making progress on signing osquery CCLA (was blocked for a few months)
> Follow up is manually syncing changes (TODO: Ted).

We don't know what this means. 

## Extension SDK

https://github.com/osquery/osquery/pull/5851 is up. This returns to the old 3.x state. Folks are reviewing.

This is awesome!

We've discussed whether we should move the go SDK to the osquery from the Kolide org to the osquery org. This pending conversations with Kolide.

## Packs

https://github.com/osquery/foundation/issues/28

A sprawling conversation, very much aligned with the aforementioned issue. 

Several points got raised:

* Project maintainers and core members view the packs as rough examples.
* Packs are out of date.
* Packs may be non-performant or useless.
* End users often view the packs as all they need, batteries included.
* They're often seen as "Facebook approved" and being all you need for security.
* Does the availability of these do more harm then good? Does it carry an implicit guarantee?
* Maybe make the packs smaller?
* Changing pack details is hard. Changing a query can cause a schema change on the output. It can also trigger epoch issues. How does one version packs?
* Packs were always the most accessible part of osquery. You don't need to know C++. You can be excited about threat tracking, and jump in.
* We need better user education. Some kind of quickstart. How to get going. Query examples.
* Packs also highlight another common discussion -- What is the point of osquery. Some people use it as to extract data to an SIEM. Some use it as a way to get a clear indicator of something wrong. Some use it as a way to gather basic machine facts. These different approaches have big differences in what kind of queries people want.
* Differenciate between a useful pack that's ready to do. Versus a example/documentation aimed at educating people in how to do their own things.

General consensus that we should pull the packs into their own repo. We may still include them in the shipped osquery products, but having them in their own repo gives us a place to encourage collaboration and evolve from.

seph proposes denormalizing into fleet's yaml format. Primarily because seph likes denormalized things. No clear objections here, as long as we still checkin the build artifact.

## GitHub Reviews

Proposal to disable GitHub "Dismiss stale pull request approvals when new commits are pushed" to help us move faster on merging PRs (Ted).

* seph likes this feature, he finds it a good sanity check.
* Stefano likes this feature. It helps ensure that the changes are reviewed. Without it, it's very easy for people to miss that there are updates.
* Mike likes this feature. While there's annoyance, that annoyance should be relative to the size of the new change. Eg: if you need a new review for a small fix, it should be easy to re-review. 
* groob doesn't like this feature. He thinks it causes friction. A PR can get to a point where we say "It's fine to merge, fix this one trivial error". Then GitHub than requires a re-review, which is can be slow.

It does not sound like there's obvious consensus. So people want to advocate for it, make an issue and we can discuss and vote.

## Build Infra

We've been having some discussions on Slack, issues, etc. Do we like CPack?

Right now, we think we like CPack enough. CPack has some benefit in not having a lot of dependencies.

When we confirm CPack is working, we should remove the old scripts. 

Pending issues:
- https://github.com/osquery/osquery/issues/5769

