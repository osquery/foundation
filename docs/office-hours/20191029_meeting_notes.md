# osquery office hours 2019-10-29

YouTube Recording: https://youtu.be/z3mEHMvepgA

## Announcements and Highlights since the last meeting

* Lots of PRs for the build process
  - Work around for the git submodule issue (https://github.com/osquery/osquery/pull/5892 https://github.com/osquery/osquery/pull/5897)
* Fuzzing getting integrated (Thanks @tomrittervg)
* Several more tests enabled(Thanks hacktoberfest!)
* Nick helped us out with some of the Buck issues!
* Extensions

## Any PRs people want to discuss

(The intent of this section, is to allow contributors to bring up and PRs they feel blocked on.)

### There are PRs for new Windows tables by puffyCid.

https://github.com/osquery/osquery/pull/5831
https://github.com/osquery/osquery/pull/5710
https://github.com/osquery/osquery/pull/5539

These are somewhat substantial PRs. Probably need reviews both from
the c++ side, and from a windows API side.

Nick (@muffins on GH) says he's happy to do some windows reviews, but
often needs to be prodded.

### Events Forwarding (@uptycs-nishant)

https://github.com/osquery/osquery/pull/3482

Some conversation about what's happening here. Is Facebook working on
the events stuff? They started some work on ev2, there is no work
that's happened off github. (It's sounds like this work is a bit on
hold)

Alex mentiones that bypassing along isn't a big win, since you still
need to cache the events. He's got a couple of upcoming PRs that help
with this path.

Nishant comments that you may not need to run through the sql staging
area, instead you might want to push the full event stream
remotely. This approach does not allow for conditional contraints on
what's forwarded. Though Uptycs may have some filtering in their
config file.

seph (@directionless) comments that this sounds like it's getting
_away_ from the underlying sql model of osquery. This feels like
something that should have an blueprint and conversation about how
architectural vision looks like. (ev2? CEP?) Some discussion about
whether or not sql is core to what osquery is.

Further discussion about whether event data even makes sense inside
the context of sql. Joining has an inherent race condition. But we
think that filtering is a major use case.

https://github.com/osquery/osquery/issues/5966

## 4.1.0 Release

https://github.com/osquery/osquery/milestone/43

This was originally slated for the end of the month. How are we feeling?

### Python tests

https://github.com/osquery/osquery/pull/5836

Lots of progress here. Some issues though. The python tests are finding osquery bugs, which require iteration.

One weird one is about CI failing to read a json test file.

### CPack/packaging Stuff

https://github.com/osquery/osquery/issues/5769

Our goal is to try building packages inside cpack, and not using the older osquery packaging scripts. 

2 more PRs left before the CPack/packaging is completed. One is blocked due to needing more CI triage.
 - https://github.com/osquery/osquery/pull/5951
 - https://github.com/osquery/osquery/pull/5936

Nick asks if we've moved the MSI packaging to cpack. We think the MSI
packaging is using cpack, but we probably need to update the
chocolatey process.


## Developer happiness

### inconsistent include paths

In one future state we do not export headers outside of the `./osquery/include` directory.

(Teddy is probably going to take a pass at this)

### exported headers

Can we move from `<>` to `""`, remove `./osquery/include` directory or
define best-practices? We discussed this last office-hours (sort of)
in the question about our public API. It is generally confusing to see
everything included with `<>` system-first search order. (latest work in
the experimental branch was moving from `""` to `<>` - Alessandro)

(Teddy is probably going to take a pass at this)


### Link Dependencies / 'Series-of-linkage targets'

If you look into CMakeLists files there is a static list of linkage
targets for (every source file). Can we change this practice to have 1
list of linkage targets per-folder? For example, all files in the
./config folder mostly share the same linkage targets.

This is related to https://github.com/osquery/osquery/issues/5916 (which is a larger issue)

(Teddy is probably going to take a pass at this)


### Test refactor

'Per-directory tests', If you look into CMakeLists files, for ./tests
folders, we create an executable target for each file.cpp. Can we
switch to compiling a files in a ./tests folder into a single
executable?

This has a potentially issue around options. What if things need
different options? This might be fine if we do it per cmake file. May
also require tests running with different permission.

Some discussion about how we have:
* test binaries
* which contain tests
* which contain test cases

(Teddy is probably going to take a pass at this)

### Does anyone use Buck

It sounds like is helping both Nick and Teddy

### Are windows builds taking much longer? (asks @muffins)

I've noticed Windows builds, esp clean builds on my local system, are
_exorbitantly_ long and I'm curious if anyone else has noticed this/if
it's expected? A lot of the build looks related to AWS SDK artifiacts,
just wanted to bring this up and get feedback lot of the build looks
related to AWS SDK artifiacts, just wanted to bring this up and get
feedback

(This is mostly due to the fact that we are building with MSBuild;
seems like NMake would have roughly the same performances. Using Ninja
would improve compile time quite a lot, but we are currently
experiencing issues in the generated binaries. We would like if people
could help us out debug the issue. Testing it is easy: from the VS
prompt we can just pass `cmake -G Ninja .. && cmake --build .`. Issues
could be just caused by the fact that we are currently mixing
source-based modules with pre-packaged libraries from the
`libraries/cmake/facebook` layer - Alessandro)

This is not unique to windows, but on linux we use ccache. Stefano
says this is due to the underlying ec2 library, it takes 4 gigs of
ram. Because the CI machines are small, this ends up being really
slow.

Alessandro says Ninja would help with this. This is because it
supports partial builds. (Buck does partial builds)

seph asks if we can use the dependancy layers for this. Answer is that
while we can do this, using ninja and ccache sound more desirable to
the cmake people.

* Add ccache -- https://github.com/osquery/osquery/issues/5967
* Add Ninja -- https://github.com/osquery/osquery/issues/5968

## Foundation Updates

### Money

To start testing money flow, [seph](@directionless) and Alessandro
have tried various donations to osquery via [Community
Bridge](https://funding.communitybridge.org/projects/osquery). Some
worked, so we do have a means of accepting money. We have not to spend
it. Actual billing always happens at the start of the next month, so
when using prepaid it's best to have enough money on it for everything
attached to the card!

Alessandro reported some issues with international credit cards.

We've reached out to the PM for Community Bridge Funding to ask about
international cards and paypal

We should link to it from the web site https://github.com/osquery/osquery-site/issues/154

### Apple Dev Account

https://github.com/osquery/foundation/issues/3

We've been exchanging some email with Michael at the LF about DUNS
numbers and dev accounts for certificates. No conclusion yet, but
some progress.

## Question: are there killswitch users? There is a PR to remove this 'unused' feature.

https://github.com/osquery/osquery/pull/5949

Are there killswitch users? There is a PR to remove this 'unused' feature.

Sounds like no users in office hours. We think that people see the PRs going by.


## Short overview of notarization and how it affects osquery going forward

Folks asked for information about apple's notarization. Seph can give
a quick overview. There's a some additional notes in
https://github.com/osquery/osquery/issues/5894 (Note that this is
seph's understanding. This is not apple policy)

Apple has several layers of things that help protect end users. This
are generally all enforced by the gatekeeper process. Gatekeeper runs
against things that have the quarantine bit set. This bit is set by
various download style things. (Web browsers. airdrop, etc). As a
power user, you can always remove the extended attribute for testing,
but we should be notarizing.

Codesigning has been around awhile. This process requires someone with
a apple dev account to sign the binaries and packages. This
verification is inherently local. "Is it signed by a valid cert"

There's a new requirement, "notarization", which is about submitting
the binary/packages to Apple for an offline malware scan. This is an
automated process. There do not appear to be humans in the
loop. Though it is asyncronous, it's been under an hour.

The notarization check is **not** local. It's a call against apple's
verification servers. However, you can download a certificate of
notarization, referred to as a "ticket" and staple it to the "thing".

seph is a bit fuzzy on exactly what "things" can be notarized:
* Apps must be, and support stapling
* macho binaries must be, and do not support stapling
* dmgs? Maybe
* zip files can be (this notarized all the things in the zip)

Do we have an ETA?
- Currently applied only on things downloaded by the internet
  (enforced with an xattr - quarantine bit)

In January 2020, the requirements for notarization become a bit
stricter. (well, they revert to what they were prior to apple relaxing
them). Notably, we must codesign to use the hardened runtime. We
should test that things work

