# osquery office hours 2021-04-13

YouTube Link: https://youtu.be/DrQ9LoAMKEg

## Announcements and Highlights since the last meeting

## Any Questions / Issues / PRs people want to discuss?

The intent of this section is to provide a clear time for community members to bring up _anything_.

Broad questions? Bugs? Deployment questions? Blocked PRs?

### Some hard to reproduce issue with event optimization

Someone mentioned a report of a hard to diagnose bug around event optimization. 

### osquery enroll stuff

(Several related things)

https://github.com/osquery/osquery/issues/6887

Some discussion about what this is actually about. Consensus of office hours is that if osquery is configured to use a TLS server, and cannot enroll, then there should be no fallback to standalone mode.

There's an unrelated issue where enrollment can happen in multiple places. Which means the `max_attempts` option doesn't  apply as expected. This is because the enrollment is happening inside the `get_node_key` function, so if multiple threads call it, they all try. Refactoring out enrollment would make sense. Zach wrapped it in a mutex, which is a bit of a bandaid, but might work.

Another issue -- during enrollment, the worker doesn't exit on stop signals. These are caught by the parent, and the worker does not check.

## 4.8.0 / 5.0.0 Milestone reschedule

https://github.com/osquery/osquery/milestone/54

May 1? June 1? June 1 and we will maybe make it 5.0 if the new signing pipeline, ARM CI, and EndpointSecurity gets merged.

Alessandro, Sharvil, and seph will try to meet and get the signging pipeline done

## PR 7048: security assurance doc

https://github.com/osquery/osquery/pull/7048

Does anyone have suggested topics or bullets I can add to this, before starting? (mike)

seph will try to review

## PR 7046: EndpointSecurity based process events

https://github.com/osquery/osquery/pull/7046

Needs review

## osquery distributed as an .app bundle (macOS)

Background: For an entitled (and codesigned) binary on macOS, making use of restricted entitlements (like EndpointSecurity), the binary needs to be provisioned using provisioning profiles. For the system to automatically pickup that provisioning profile (in macOS parlance called `embedded.provisionprofile`), the binary needs to be in the macOS .app bundle format.

Sharvil has tested this manually, by creating the correct structure and app bundle layout, with the said `embedded.provisionprofile`, and the location of this app bundle doesn't matter.

The usual location of `osqueryd` -- `/usr/local/bin/osqueryd` can be a symlink to `/usr/local/bin/osquery.app/Contents/MacOS/osqueryd`, which works and preserves expectation and backwards compatibility.

Ideally, other files (Augeas lenses, but not the things from `/etc`) would move into the .app as well. What is an example of how this should be implemented? Santa? This "best practices macOS app bundle usage" can be a follow-up if it is substantial effort.

Discussion is about whether or not we should embrace `.app` and move everything into it. Or setup something minimal to get the provisioning profile working or what. Conclusion was that we should merge the implementation of EndpointSecurity first, to not hold up any further, then address the app bundle as a deployment requirement before the next release.

## Look at old PRs 

_(If there's time, we've been trying to re-visit old PRs)_

[Reverse Sorted List of PRs](https://github.com/osquery/osquery/pulls?q=is%3Apr+is%3Aopen+sort%3Acreated-asc)
