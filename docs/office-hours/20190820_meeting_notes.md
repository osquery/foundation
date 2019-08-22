# osquery Office Hours 2019-08-20

[YouTube Recording](https://www.youtube.com/watch?v=sSpoM1ebYWc)

## Highlights since the last meeting

* Third Party PR is open -- [#5706](https://github.com/osquery/osquery/pull/5706)
* New Tables & Improvements
  * We now have a new `ibridge_info` table on macOS that reports the available T1/T2 chips on the system. Thanks @sharvilshah!
  * Certificate support on Windows has been greatly improved, and now better supports (system) users. Great work from @mossberg
  * The mounts table could sometimes cause filesystems to get mounted. Thanks for fixing this @emptymonkey!
* Packaging scripts now support Buck! Thanks @muffins!
* Testing
  * The address/undefined clang sanitizers that were originally added by @ggrieco-tob have found several errors. Thanks to @smjert for fixing the majority of them!
  * We moved to the openjdk8 brew package when installing Java on macOS. This replaces the deprecated package that we were using before. Thanks @tchinmai7!
* Thanks @directionless [#5445](https://github.com/osquery/osquery/pull/5444)

## Steps to a stable release

Third party libraries built from source (PR #5706)!

* The majority have been converted. Only OpenSSL is being built with its original makefiles.
* Some fixes for Windows are needed.
* Some optimizations missing to improve CMake configure time.

The configuration files are committed. This should help with deterministic builds. The custom toolchain folks are working on will help this work more correctly for glibc. (This targetting the end-of-August release.)

The dependencies toolchain is Linux only right now. While we expect to expand it, not for next release. 

Signing and distribution is still a bit ad-hoc. We expect Facebook is still willing to sign binaries. We expect to release to GitHub. We are not sure if facebook will copy into the download bucket. We generally seem unconcerned. While there is a lot to improve here, we do not have to tackle it all at once. 

### Custom toolchain to build a portable osquery

* Is almost ready, needs to be tested against osquery and the new third party libraries system.
* Signing?
  * Let Facebook do it.
  * What does the build? Will Facebook do the build? Will they want to take it from CI?

### Next Steps

* Needs polish
* Compilation time is slow
  * There is a lot of cloning happening.
  * Shallow clones, avoid what we do not need.
* How can we help?
  * Folks should build & test.
  * Dependancies are only enabled on Linux, so that is the most valuable.

## Release Focuses

[4.0.2](https://github.com/osquery/osquery/milestone/42) is the end-of-August, the main thrust there is the new toolchain.

[4.0.3](https://github.com/osquery/osquery/milestone/43) is next. Focus there is going to be on getting the extensions working. That's blocked on the toolchain.

## Changelog

Discussion about whether we can/should make a changelog for the release. Doing this would be a big win for marketing -- it helps sell the osquery release, and promote what we are doing. 

There's general interest in seeing this. Discussion about wanting to include things. @sharvilshah said he would try it.

## Foundation Process & Procedures & website things

@directionless is trying to make a set of foundation process and procedure docs. Nothing very far here.

@zwass offers to make a foundation section on the webpage. Should be an easy first pass of office hours, and high level things. 
https://github.com/osquery/osquery-site/issues/145

## Finances (misc items)

### What are our goals?

Do we want to solicit money? Things like https://help.github.com/en/articles/displaying-a-sponsor-button-in-your-repository

Do we want to thank people, who are contributing hours? (Facebook, and Trail of Bits)

These will have different answers. We may want to make a manual list on Github to thank some of the topline people. We expect to keep evovling.

### ReadTheDocs ads

Right now, there are some ads on the osquery part of ReadTheDocs. 

ReadTheDocs reached out to us to ask if we wanted profit sharing, and to tell us there's some interest from vendors running ads. 

We do not yet have all the info we need here.

### Linux Foundation and money

We have started talking to the Linux Foundation about managing money.

They pointed us at https://communitybridge.org, a tool they use. 

## CLA

The new CLA uses docusign, and requires an address. This has been raised as a question, may or may not be a concern.

@directionless reports that the Linux Foundation went this route on the advice of their lawyers. And that they had some comments about CLAs not having been tested in court.

Maybe some people will try to talk about it this week at OSSCON in San Diego. (Unclear, this was a comment on slack)
