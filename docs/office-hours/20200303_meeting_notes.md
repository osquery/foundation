# osquery office hours 2019-03-03

YouTube Link: https://www.youtube.com/watch?v=ELhVU8R8q4k

## Any PRs people want to discuss

### Batching database writes in the Windows events tables

Alessandro has started some work on Windows eventing
batching. https://github.com/osquery/osquery/pull/6280

A bit of digression with Nick about [cosine similarity in the powershell_events table](https://github.com/osquery/osquery/blob/ae66d8f3bc367f9300fb83213c9e762989c34fa5/osquery/tables/events/windows/powershell_events.cpp#L73). How
to use it. Speculative conversation about what other statistical
functions could be interesting. (bloom filters? Hyperloglog?)
Discussion of sql functions in core, vs extensions.

## 4.2.0 Released

Do we want to push the Linux packages to their repos?

Several of us have tested the binaries, but not the packages.

## 4.2.1

Someone asked on Slack that we set a due date, so I picked 2020-05-01,
about 2 months after 4.2.0.

## macOS packages

The macOS package is missing the postinstall
script. https://github.com/osquery/osquery/issues/6282

Highlites the need to get this better automated. 

### Would we have any benefit from using a app bundle?

App bundles are needed for:
* UI driven work
* Events
* TCC permissions are often app bundle driven
* the endpoint security needs a provisioning profile. Which needs a
  filesystem structure, which is most of what an app bundle is. [apple's
  doc](https://developer.apple.com/library/archive/technotes/tn2206/_index.html)

It's not yet clear that we need one. Though the endpoint security is a
compelling reason. This is likely to change at some point.

## Documentation

Is it possible to have the README include a version specific
readthedocs link?

It's not clear this can be automated inside github. But we could link
to the list of available doc versions. Or make it part of the release
process.

https://readthedocs.org/projects/osquery/ seems to be the link to the
available version

This has been done. https://github.com/osquery/osquery/pull/6283

## Community

### How to publicize features

We have cool features. How do we publicize them?

We used to have a good cadence of blog posts. That gave us a good
place to write stuff, and retweet from. Can we get back to that.

Suggestion to pick a feature, and write a paragraph about it, and put
it in a blog post. We may be able to use (or link to) Zach's writeups.

Discussion about how to add this to the PR process.
* Does it make any sense to tag PRs? What are the expectations for
  that? Will it work or not
* Can we make "write a paragraph" part of the PR process
* We might also want to encourage people to write docs as part of the
  their PR
* Ben points out that some folks are willing to write, but not code
  (he points at himself), so how do we communicate what we'd like
  writing about. Maybe slack? Maybe a new channel? Maybe an existing
  one? #release-notes now exists. We'll try it.

### Blog activity

Discussion about the blog on the web site? What's our blog for? It can
help people see how widespread osquery usage is. Questions about
whether we want to encourage people to cross post. And what would this
mean?

Apparently it's hard for people to cross post from other systems,
since it needs to be in markdown. In addition, there's a push and PR
process

Would there be value in posting a summary and link to other people's
blogs?

If we're trying to drive the community view, maybe default to showing
the community blog, not the official one?

## External links

* Here's a really cool way to correlate Zeek events with osquery from
  Zach! https://twitter.com/TheZachW/status/1233102349507252224

