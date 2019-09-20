# osquery Office Hours 2019-09-03

YouTube Recording: https://youtu.be/urMFR1ICFbU

## Highlights since the last meeting

* Lots of progress towards [4.0.1](https://github.com/osquery/osquery/milestone/42).
  - (This has its own agenda item)
  - Changelog merged (big thanks to @sharvilshah)
  - Third party libraries merged (https://github.com/osquery/osquery/pull/5706)
* [Views created on startup](https://github.com/osquery/osquery/pull/5732).
* Several documentation updates. Build instructions, and Windows packaging.
* Additional tests for tables.
* Remove FTS features from sqlite (in response to vulnerability) (See https://github.com/osquery/osquery/issues/5702 for background).

## 4.0.1

### Third Party Libs

https://github.com/osquery/osquery/pull/5706 merged. This means third party libraries are now building from source on Linux! Huge work from @alessandrogario, @smjert, @theopolis. The plan is to adopt the 'layer' approach to macOS and Windows after 4.0.1.

### Toolchain

Custom toolchain to build osquery is not yet merged. The toolchain is ready, and seems to work. You can build with it, and tests pass. But integrating it with the third party dependencies is surprisingly hard. This is partly due to the age of the CentOS6 libc, and how it works with the third-party libs.

@smjert is working on this, with help from @theopolis.

We do not need to support _building_ osquery on CentOS6. But we do need to _run_ it there. The build/test process right now, is to build on Ubuntu, transfer the binaries to CentOS6 and then test. (As it happens, the build system does work on CentOS6.)

An example:
* augeas needs `fnmatch`
* The `fnmatch` in the older libc is buggy
* So augeas has a `posix_fnmatch`
* But autoconf will not detect it when `./configure` is run on newer system -- autoconf will make the best decision for the running environment, we need it to make the most portable decision.

Discussion about shipping a libc? Conclusion is that this is not something worth pursuing.

There are some concerns about merging the toolchain work. As it stands, it probably breaks build and runtime on some older platforms. But, the current build uses the OS toolchain, which is also broken on those platforms. So merging is not worse.

What if we limited the supported platforms? Would this let us ship faster? What would our messaging here be? Does anyone use CentOS6. We believe that we are as stable as in prior releases. But we understand more about third party libraries, and we think there are places we can improve for older versions of libc.

More people to help update third-party libs would help. Stefano has done about seven libraries. We have about 40. Not all libraries are equal.

#### How do we move forward?

We **will** merge the custom toolchain work. This needs a small amount of doc work, and binaries somewhere (osquery/osquery-toolchain repository). https://github.com/osquery/osquery/issues/5609

_Then_ we will tackle the third-party dependencies one at a time, getting support for older libc into their configuration files. This work will be done a third-party lib at a time. https://github.com/osquery/osquery/issues/5745

Future work around better automation & CI. This may look like a chroot or a VM for the autoconf stage. https://github.com/osquery/osquery/issues/5743

### Package Signing

## Side discussion about our users

There's some discussion about whether anyone uses CentOS6, and more broadly, what our user base does.

@theopolis has some ancedotes about why an older libc is interesting, primarily around embedded platforms and routers. He'll try to dig those up.

This is seen as a larger topic for another time.

## Roadmap

### 4.1.0 -- Extension Support

Note that in the discussion we referred to 4.1.0 as 4.0.2 but this milestone has been renamed.

https://github.com/osquery/osquery/issues/5636

The main issue is that to ship extension, we need the custom tool chain and dependencies. So extension support was pushed forward to 4.1.0.

### Future work

We should have a roadmap. Some comments:

* The metaprogramming around the spec files https://github.com/osquery/osquery/issues/5502
* Stronger typing
* A bunch of work from @packetzero

How do we talk about this? @mike-myers-tob suggests we use the GitHub project kanban tool for this. It's somewhere to center a discussion. And it's something supported directly in GitHub. We could use multiple boards, reflecting different time periods.

Let's try it. @directionless will click some buttons to set it up.

## Finance / Fundraising

This is https://github.com/osquery/foundation/issues/15

The readthedocs people reached out to us about profit sharing, which means we need a bank account. The Linux Foundation pointed us at Community Bridge, a platform they're building out.

@directionless went through the signup process, and we are now live at https://funding.communitybridge.org/projects/osquery It all looks a bit rough right now. But, we can probably take donations in, and the TSC can claim expenses.

### Can we become self sustaining

We had a small conversation about what it might mean to become self sustaining.

Right now, various people and organizations are providing resources. An incomplete list:
* Azure
* gsuite
* S3 Hosting
* Slack
* Time and Humans

Some additional discussion about what kind of funding models might make sense. Would we fund an engineer to work on something? This is currently something to think about pending future discussion.

## Core Infrastructure Initiative -- Gamification of Best Practices

This is https://github.com/osquery/foundation/issues/17

https://bestpractices.coreinfrastructure.org/en/projects/3125

Funding (Community Bridge) required us to apply for a badge within 15 days. It is not concrete if we need to complete; we are making the assumption that we are not expected to complete requirements of said badge.

Adding the badge to the README.md -- https://github.com/osquery/osquery/pull/5744

## How should we approach old PRs?

We have a mountain of PRs and issues. How do we get those moving forward? A start would be labeling the backlogged PRs and issues.

@mike-myers-tob has done some review, and wishes for more labels. He mentions he doesn't have the rights to label things. Can we fix that?

@theopolis and @directionless have started reviewing some PRs and tagging the original authors for rebasing.

Question about whether or not it makes sense for us to do the work to integrate ourselves? (Instead of waiting for the original author). No conclusion. But be aware of the CLA needs, which may be covered if they signed the old Facebook one.

