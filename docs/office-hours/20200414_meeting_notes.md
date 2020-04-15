# osquery office hours 2019-04-14

YouTube Link: https://www.youtube.com/watch?v=JlV3YkJ5rDI

## Announcements and Highlights since the last meeting

## Any PRs people want to discuss

### Add status and statements to distributed query results
https://github.com/osquery/osquery/pull/6352

This came up last week. The PR includes both statuses and statements,
and there's been some discussion about whether the statements are
needed.

Some discussion about this, is that there's general hesitency. This is
probably stemming from queries being very large, and that it should be
already available to the server. This can increase log storage, and
network requirements.

Maybe a middle ground would be to only include it on errors? More
discussion needed.

One approach would be to split the PR into two. Status is widely
agreed on, and can be merged. And we can keep discussing the
statements?

### Implement event batching support in Windows tables
https://github.com/osquery/osquery/pull/6280

On Nick's plate. Nick and Alessandro will meet to walk through the
changes.

### Implement container access from tables on Linux
https://github.com/osquery/osquery/pull/6209

Zach had approved, but the rebase cleared that. Should be an easy
re-approval.

ToB is also excited to write a blog post for this and the related
table improvements in PRs blocked by this one.

### HTTP Events table
https://github.com/osquery/osquery/pull/6366

We want to encourage Uptycs to contribute, and make it reasonable for
them to merge their fork back. They submitted this, how do we feel
about it? We should decide on whether the functionality makes sense,
even before we go back and forth on the implementation.

Several concerns raised:
* Processing network data from packet capture, in osquery's root
  context, is unsafe. Though we do parse some user controlled data,
  this feels like a big increase in attack surface. For example,
  wireshark has had a number of vulnerabilities about this.
* This table merges a lot of functionality. http headers and JA3
  fingering printing should be split.
* This implementation doesn't do re-assembly, so it does not correctly
  handle pipelined requests

After some discussion, we agree these are concerns, but we'd love to
hear Uptycs speak to the underlying motivation. Hopefully they can
come to the next office hours. Zach and Mike will try to followup.

This highlights a bit of a gap in how we document some of our policies
and goals about what kind of code belongs in osquery.

## Build system without Buck

When `osql` first brought cmake in, many compromises were made to be
compatible with Buck. Now that we have removed Buck, the cmake can be
simpler.

Some discussion about how this translates to depenancies. It's
positive to have explicit defines for each target. But, that makes it
hard to use an in IDEs. Also, the folder structure isn't on disk,
which makes using external tools harder.

One thing we can do now, is to start refacoring names to be
shorter. This should be able to happen aside from any include
organziational questions.

## 4.3.0 Release

Stefano has started this.

New Release checklist at
https://github.com/osquery/osquery/issues/6386

Beta packages on #core

Teddy has signed macOS. Nick will look at windows tonight.

## Github New Pricing Structure

osquery's billing page says we're still on Facebook's legacy Bronze
plan.

Github has announced a new pricing structure
(https://github.com/pricing). They have expanded the free tier, we are
unsure if this helps us.

There's a lot of discussion and confusion about this:
* We're not wholly sure what plan we have, and whether we're blocked
  on anything
* We do think we should move out of Facebook's enterprise accounts
* Some features we want: 2FA, "required reviewers". These are probably
  now in the free plan, but it's a bit unclear.

Both Teddy and seph pinged some email threads with GitHub.

If there are github features we want to use, we should just be trying
to use them. Actions to automate the website deployment came up.

## Foundation Update (DUNS Number, codesigning certs)

seph has been working through getting code signing moved from Facebook
to osquery. One of the things needed for an organizational Apple
Developer Account is a DUNS number. Last week we finally got one.

There's a brief waiting period, and the next steps are to submit to
apple for a organziational cert. Apple has some programs for OSS and
non-profits, this may not have a fee.

See: https://developer.apple.com/support/membership-fee-waiver/

Windows is future work.
