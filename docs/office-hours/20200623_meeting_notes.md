# osquery office hours 2020-06-23

YouTube Link: https://youtu.be/eWDUlOW8Ulw

## Announcements and Highlights since the last meeting

## Any Questions / Issues / PRs people want to discuss?

The intent of this section is to provide a clear time for community
members to bring up _anything_.

Broad questions? Bugs? Deployment questions? Blocked PRs?

### App Armor Profile

https://github.com/osquery/osquery/issues/6484

What could be locked down? We could start by allowing full read
access. What kinds of things might it never need? Are there specifics
it would lose access to? How would this work for forensics or IR?

There is some prior art in osquery around dropping privileges. For
example, we drop privileges when we access the rpm or deb, to keep
osquery from unintentionally modifying state.

Maybe we can limit filesystem write access?

Can osquery run as non-root? Someone on slack does this, it might be
another route.

Probably next step is to put up a proposal, and try to see what we
think of it.

AppArmor's complain mode might also be useful for discovery.

### Questions based on constraints

https://github.com/osquery/osquery/issues/5503

A question from the community about what columns are required, and how
that translates into where clauses. Something about `left joins` no
longer working.

If it's the `file` table throwing warnings, it's probably not an
osquery row. Speculation that maybe some rows are missing the path.

General conclusion is that we're happy to help debug #sql, slack or are
the best route for that.

## Agenda down here

## Windows Tests Failing

There may be multiple failures in play.

Stefano started looking at one around #6507 Seems to be something in
the http constructor. Somethine about a deadline timer internal field
being null. Master does not crash.

Another is related to the process
uptime. https://github.com/osquery/osquery/issues/6395
specifically. While we can defer solving the larger "what does uptime
mean" question, we probably should fix the tests.

## 4.4.0

https://github.com/osquery/osquery/milestone/49

This had been waiting on https://github.com/osquery/osquery/pull/6280
Alessandro thinks it's mergable now, and may want a followup PR
explaining how the test data was generated. Conclusion -- merge now.

We think it's worth holding 4.4.0 for some of these merges. There is a
flaky test, but some ci failures that might be real. So we think:
* We should disable the flaky test (pending a real fix in #6395)
* If we can positively confirm CI flakiness, we can merge over that

Needing code review (CI tests are flaky)
- https://github.com/osquery/osquery/pull/6513 (flaky test + code review)
- https://github.com/osquery/osquery/pull/6512 (needs code review)
- https://github.com/osquery/osquery/pull/6507 -> Move to 4.5.0
- https://github.com/osquery/osquery/pull/6492

Should we land and follow up PR with tests?
- https://github.com/osquery/osquery/pull/6508

Should we land and follow up with documentation?
- https://github.com/osquery/osquery/pull/6280 (merge it!)

## WWDC

Apple WWDC 2020 has started. Several notable things for osquery.

New OS. Codename Big Sur. Initial testing:
* osquery runs
* `asl` table works, despite being deprecated

New Hardware -- Apple Silicon:
 * https://github.com/osquery/osquery/issues/6517
 * Likely the same as the iPad. 
 * seph has requested that the osquery project get developer access. 
 * Apple has probably batched boost and cmake (so this is probably fine)
 * Unclear if we can cross compile, probably
 * We can probably use `lipo` to make universal binaries

Check back here on Wednesday, there will be a presentation on
developing EndpointSecurity clients:
https://developer.apple.com/videos/play/wwdc2020/10159/ They're adding
support for some new events related to filesystem extended attributes
(xattr).

## "Focus" for osquery development

Proposal to focus our efforts, data integrity and accuracy, bug fixes,
and... 'cloud first'!?

Can we create user stories to help drive what we need here.

Let's mull on this, and then figure out the next steps. Maybe user
stories into git.

