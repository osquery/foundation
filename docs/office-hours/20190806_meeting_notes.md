# osquery office hours 2019-08-05

## Highlights since the last meeting

* CLA is in place
* `users` table regression fixed (Thanks Teddy & seph)
* Myriad of build updates (Thanks Teddy, Stefano, & Alessandro)
* Many dependency have been converted

## Action Items

## 4.0.2 release status

https://github.com/osquery/osquery/milestone/42

Blockers are the toolchain and dependency issues. Alessandro feels
okay with the end of august.

End-of-July pre-release? Maybe. There's toolchain work around centos6. Probably makes sense to defer it.

Version in buck. Can probably call external commands, but we can probably not block on it.

## Extensions in 4.0

It really blocks on the toolchain. So, we think we should defer this to 4.0.3.

Alessandro has looked at it, but isn't picking it up now.

## Docs / Readthedocs / website

We can update docs, there are hooks on a repo that update readthedocs -- https://github.com/osquery/osquery/issues/5688

We can update the website. PR to https://github.com/osquery/osquery-site  and manual deploy with `yarn deploy`

But we also need to untangle ownership on readthedocs -- https://github.com/osquery/foundation/issues/11

## Table caching

seph says that while debugging the `users` regression, he noticed that
there's something amiss in how virtual table caching is supposed to
work. He's seeing the `gen` functions called repeatedly. This is being tracked in https://github.com/osquery/osquery/issues/5668 and is likely related to https://github.com/osquery/osquery/issues/5675

packetzero did some debugging around this,
https://github.com/osql/osql/pull/12 was the result. This looks
promising. Should revisit.

## Non-system toolchain builds on Linux

Custom toolchain based on Teddy's buildanywhere work.

Some issues, but Stefano is confident that's is working.

Extra weirdness around cross building gcc. 

No help currently needed, he's fighting and merging things.

## Status of third-party redesign

https://github.com/osquery/osquery/issues/5652

Most libraries that can be converted are ready. Currently only enabled
on linux to minimize testing surface.

A draft of openssl is up, 4 remaining. Feels okay for the end of the month stable.

Feel free to try out the branch. Some tests are breaking, so looking
at that could help. Normal build process.
1. checkout
2. cmake
3. make
4. ctest

## Publishing packages

https://github.com/osquery/osquery/issues/5689

Azure can use cpack and push places. So we've got options.

seph really wants us to think about build separately from packaging. They're different pipelines.
* build
* package
* sign

Stefano says the existing scripts are all part of the cmake stuff. They can run standalone. 

Discussion about how to sign things. And how key management
works. We'll want to keep the keys in a KMS somewhere, but we might
need to keep a key in a keychain. There's some prior art in doing this
for microsoft's signtool.

There's some complications about an EV cert.


seph mentioned some interesting blog posts. Here's a small link dump:
* [osslsigncode](https://github.com/develar/osslsigncode) is a barely maintained tool, but it does sign windows binaries
* There are hints about using signtool with cloud HSMs
  - https://docs.aws.amazon.com/cloudhsm/latest/userguide/ksp-library.html
  - https://vcsjones.com/2017/05/07/custom-authenticode-signing/
  - https://vcsjones.com/2017/12/14/azure-signtool/
  - https://github.com/vcsjones/AzureSignTool
  - https://natemcmaster.com/blog/2018/07/02/code-signing/

## Flaky windows buck tests, timeout?

It's not sure we have enough info with the people present in this meeting

seph says the tests are slow, as compared to his laptop. This might be because the VMs are small. 

## Closing all open / stale issues by end of September or October

We think it's a good idea. But it they need individual evaluation.

This might fit well with the prior suggestion of having allocated time
to review things?

## Syncing facebook commits into GitHub

Facebook has migrated some of the buck libraries internally. We think this can probably co-exist with cmake.

They've got some internal fixes internally, they'd like to upstream. They're planning on doing it manually.

## Office minute taking

Right now, we're doing the agenda in google docs, but then we're
moving the minutes into github/markdown. This conversation is, at best, extra work.

Propose that we try one of the online markdown editing sites instead of gdocs. https://hackmd.io is recommended
