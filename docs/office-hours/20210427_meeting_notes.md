# osquery office hours 2021-04-27

YouTube Link: https://www.youtube.com/watch?v=LlmDIPwdnHw

## Announcements and Highlights since the last meeting

* 4.8.0 is stable

## Any Questions / Issues / PRs people want to discuss?

The intent of this section is to provide a clear time for community members to bring up _anything_.

Broad questions? Bugs? Deployment questions? Blocked PRs?

* Blueprint: Improvement of TLS enrollment retries [#7082](https://github.com/osquery/osquery/issues/7082)
  - Lots of discussion about whether osquery should ever exit or not.
  - No clear consensus on this
  - Discussion should happen in #7082
* PR for logrotate [#7015](https://github.com/osquery/osquery/pull/7015)

## New Packaging Workflow

Merge PR to remove stuff [#7059](https://github.com/osquery/osquery/pull/7059). Seems ready

Still need to come up with a way to tie osquery repo version to the packaging repo version. Discussion about how to do this. and whether it's okay to use submodules. Something feels a little weird to seph, but he sounds like he's in the minority. 

### Codesign updates (depends on packaging workflow)

PR is up for macOS, on `osquery-codesign` (private) repo. https://github.com/osquery/osquery-codesign/pull/1

## inotify PR rework

(From Teddy)

> I also have a refactor of the inotify events publisher that could use more love but would address the expectations people have like knowing when new files are added or replaced. Does anyone have interest in taking this over?

We don't see any code for this.

## Look at old PRs 

_(If there's time, we've been trying to re-visit old PRs)_

[Reverse Sorted List of PRs](https://github.com/osquery/osquery/pulls?q=is%3Apr+is%3Aopen+sort%3Acreated-asc)
