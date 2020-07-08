# osquery office hours 2020-07-07

YouTube Link: https://youtu.be/alvOggmhKZ0

## Announcements and Highlights since the last meeting

## Any Questions / Issues / PRs people want to discuss?

The intent of this section is to provide a clear time for community
members to bring up _anything_.

Broad questions? Bugs? Deployment questions? Blocked PRs?

### uint64 vs int64 (seph)

https://github.com/osquery/osquery/pull/6546

sqlite uses int64, and there is no real way to represent a
uint64. But, some OS APIs return uint64s. Should we create a general
purpose handler here?

Conclusion is no. This should not be a general purpose
thing. Knowledge how to handle that is going to be table specific, and
should be left in the table's implementations.

### A generic scan engine callback

ToB wants to discuss adding a callback mechanism to allow an
(arbitrary) scan engine to security-scan, e.g., files returned in a
query, providing an additional column like `scan-result`. To make
things like the `yara` table, without osquery being responsible for
signatures management.

Some conversation about whether this the same as the existing pattern
of joining against a custom table.

This was prompted by some extensions to the yara table, so
conversations about how to meet that.

A side conversation about whether it's valuable to have the
configuration file specify the resources, which would then used. Not
clear there's agreement yet.

This should become a blueprint.

## Agenda down here

## 4.4.0 Release

Any objections to declaring this stable? Let's push this live!
* github release page
* website
* linux package servers

## Container table API

https://github.com/osquery/osquery/issues/6428

> I'd like to talk about #6 in some more depth
> 
> should we see if we can contract SQLite work to make (a) select *
> from table(pid_from_namespace=10) here the missing feature is kwargs
> in the table-as-function feature, or (b) the "auto un-hidden and
> selected as part of * if in predicate" features a reality?

This is a request for conversation in the ticket, and reminder about
the discussion.

## 4.5.0 vs 5.0.0 issue labeling

Big milestones, like the internal code shift from "facebook" to
"osquery" constitute 5.0. We should keep release in 4.x until then.

## AWS Accounts

Is there someone who can volunteer to help set this up?
* spartan2194 (on slack)
* magneto (on slack)

Current state (from seph): 
* We have an AWS org structure
* It's linked to seph's personal credit card, we should be within free
  limits
* We should be using cross-org trusts
* We should be using terraform or cloud formation
* Should it link to gSuite's SSO?
* We need:
  * S3 package repo
  * CloudFront for domains
  * Route53 for DNS
  * Aarch64, maybe

Open questions about what lives in AWS vs gSuite. AWS has some nice
integration between load balancing, TLS management, and Route53.

Discussion about whether or not to apply for an AWS grant. They're
supposed to be generous, and there's some value in maybe just doing
it. Though we think we'll probably stay inside the free tier for a
bit.
* https://aws.amazon.com/grants/
* https://aws.amazon.com/government-education/nonprofits/aws-imagine-grant-program/

## GitHub Actions / Release Pipeline (seph)

With the intention of building a release pipeline, seph spent a while
this weekend playing with GitHub actions. It feels like an update to
Azure pipelines, and with Stefano's existing Azure work was relatively
straight forward to get macPS builds up and running. This work is not
yet a PR, but can be seen over in a [personal
fork](https://github.com/directionless/osquery/pull/2)

General support for GitHub actions. 

## 1Password Trial Expiration

Our 1Password account is expiring soon. They sent us
email. Confusingly, the email claims it's a trial. This is
incorrect. We have an open source account detailed
[here](https://github.com/1Password/1password-teams-open-source#osquery).

seph sent a renewal email.

## Community

### Growing the community

> idea for growing our community of PR reviewers, a 5-part class where
> Teddy teach a "how to be fabulous at reviewing osquery pull
> requests"

Both Zach and Teddy are willing to talk to folks about this. 

### Closing bugs

> we have a lot of known bugs, how do we make progress on closing
> them?
> https://github.com/osquery/osquery/issues?q=is%3Aissue+is%3Aopen+label%3Abug

Some discussion about bounty bugs as being ways to signal how we spend
money and value things.

https://www.bountysource.com is something in this space.

## Look at old PRs 

_(If there's time, we've been trying to re-visit old PRs)_

[Reverse Sorted List of
PRs](https://github.com/osquery/osquery/pulls?q=is%3Apr+is%3Aopen+sort%3Acreated-asc)

We talked about some.
