# osquery office hours 2019-11-12

YouTube Recording: https://www.youtube.com/watch?v=51dd2DpVtKc

## Announcements and Highlights since the last meeting

- Various fixes to table which implement indexes (or do not), thanks
  packetzero!
- Starting to port more and more third party libraries to the source
  layer on macOS and Windows, thanks Teddy and Zach!
- Extensions are back! Big thanks there

## 4.1 Release

4.1.0 (a pre-release) shipped. Identified some issues. Now we're looking at 4.1.1
https://github.com/osquery/osquery/milestone/45

We should probably talk about the known issues:

### Mac sdk issues -- #5993

When we moved to azure, we also started building with a newer macOS
sdk.  This means we _lost_ compatibility with older macOS releases,
notably Sierra and El Capitan. There is some intangible benefit to the
newer SDK.  We get new features (not current used), and some of our
libraries use it.

We would generally like to use the same sdk as our libraries, as there
is an unquantified risk in not doing so. One concern is undefiend
behaviour, e.g., global state usage.

After some discussion, we think we should just add the command line
flag to target the old SDK.
* This would bring back older OS support. There's value in providing
  tools with visibility on older platforms.
* The best way to fix this is to move libraries to the source layer (ongoing).
* The libraries are pretty isolated, unlikely to have shared global state.
* The risk of problems is smaller than the benefit.

### User warnings -- #6001

The current status quo is that we have code that tries to detect when
column constraints are missing. This is issued as a warning to
users. Some set of changes broke that, and now all we get is `no query
solution`. Teddy did a bunch of work here and concluded that it's not
obvious how to bring the warnings back.

We discussed this for a bit, and decided that:
* We should merge #6014, removing the warnings. As it stands, that's
  dead and misleading code that has CPU cost associates with it.
* We should investigate how to get the warnings back. Surfacing better
  errors to users is helpful.

### Extensions compatibility -- #6006

When we changed how query planning works, we stopped passing column
constraints to tables that didn't declare they implemented them. As
most of the extension SDKs lack that option, this broke most
extensions. #6006 adds a flag to pass all the constraints to
extensions. We concluded we should merge it.

But, this brought up two correllaries:

First, we should think about making a better defined API. As is, the
thrift API is very open ended. There is broad support for a blueprint
to make it strict.

Second, we do not have a clear process to deprecate functionality. We
imagine this would involve a couple releases. Warnings, and then
removal. We recognize we haven't tackled this.

## Any PRs people want to discuss

### puffyCid's work

puffyCid pinged us on
[#5539](https://github.com/osquery/osquery/pull/5539) and
[#5831](https://github.com/osquery/osquery/pull/5831)

These could use a windows level review. Thor says he'll take a look.

These, would benefit from #5901, but that's got some weird
buck/windows linking isssues. Discussion about how to move that
forward.

### Alessandro's container work

https://github.com/osquery/osquery/issues/5975

Several of us think this is great, and we'd love to see how to get
this in. (Teddy, seph)

How would we test this prior to merge? It doesn't fit well in the
unit/integration test framework. It would need some manual testing. We
presume Alessandro would be able to do that testing.

## Release Process

Teddy and seph were chatting about release process stuff --
[#6005](https://github.com/osquery/osquery/issues/6005). To summarize:

1. Tags are immutable
2. We do not have RC versions. We have numbers.
3. We cut X.Y.Z, and that's it.
4. When we decide a given tag is stable, we denote that in the github release status and on the website.

## 4.2.0

Anyone want to say something?

Some floated ideas:
* openssl update, getting everythting requires all libraries being in
  the source layer
* automating release process stuff

## Alex is leaving

Alex has a new project, and will no longer be spending much time with
osquery. He has done a bunch of work, and we'll miss him. Welcome
Vern, who will be taking over osquery at their employer.

Some discussion about moving logger caches to the filesystem. (#6012)
* Should we move all the loggers to it?
* Alienvault deployed it, and it was a win.
* Goal was to change TLS logger too, but they hadn't done the work.
* Zach will take this on.

## What private forks are out there

Some discussion about how thre's broad interesting in trying to find
the folks using private forks, and get them back to mainline. What
would it take? Are there things they'd like to contribute?

We expect some issues for commercial entities. If they've diverged,
moving their patches in is a lot of work, which is apt to get
nit-picked. And, accepting mainline changes requires careful vetting.

This is aspirational. We're not sure if there's a clear action to
take.

## Augeas motd Lens

Potential (unconfirmed) arbitrary file read with the message of the
day lens (Milan posted this in Slack). This is NOT included in osquery.

Some converation about whether we want to support this kind of
arbitrary file system read access. Right now we do not.

It's come up a couple of times in the past, for example, the mdfind
table. ([#5662](https://github.com/osquery/osquery/issues/5662))

We generally think that we cannot stop all leaks. osquery is
fundementally a tool to expose information, and there are countless
places it will leak. The registry is a good example.

Can we enumerate what data _can_ be accessed, so people can make
informed decisions?

No conclusions here.
