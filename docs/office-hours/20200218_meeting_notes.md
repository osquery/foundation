# osquery Office Hours 2020-02-18

## Announcements and Highlights since the last meeting

- Osquery 4.2.0 in pre-release, contains an important TLS CVE fix on
  hostname verification.

## Any PRs people want to discuss

- Container access from tables PR, need follow up reviews
  ([#6209](https://github.com/osquery/osquery/pull/6209))

## 4.2.0

We made pre-release packages. They're not yet code signed, but we'll
do that. See slack's #core channel.

There's a request for chocolatey packages. Nick is doing some pre-req
work around icon uploads in the packages. Nick will take a quick look
at the cpack nupkg generation. We do want to get off of the old
powershell scripts. (https://github.com/osquery/osquery/issues/6248)

## LGTM

Stefano (and a bit of seph) has been playing with LGTM for static
analysis. There's some configs in a PR.

Current state is that it bumps into the 4h time out. The machines are
2core, and not super big.

A chunk of time of the build time is in third party dependancies, so
if we had a layer that could use pre-compiled ones that might
help. Discussion about whether or not using the system dependancy
layer.

## BPF stuff

Alessandro has been working on some BPF stuff. Two initial examples

First is to hook readline. This creates events based on readline
actions. It provides a view into interactive bash sesseions.

https://asciinema.org/a/297802

Second, hooks the system resolver to generate events for DNS lookups.

Super cool stuff.

https://asciinema.org/a/302647


## TSC Updates

A bit ago Facebook proposed replacing Ryan with
Nick. https://github.com/osquery/foundation/issues/47

There's broad agreement for this. So we'll go ahead and do it.


## Community

Should we start a survey to try to understand how people are using
osquery. We're not sure how to do this, but there's interest.

Possible ways of reaching people:
* Notice on the website
* Notice on GitHub
* Notice on Slack
* Twitters

We can start brainstorming questions in a foundation ticket -- https://github.com/osquery/foundation/issues/51

## Misc

* Zach's talk "Osquery performance @ Scale" has been uploaded!
  https://www.dactiv.llc/blog/osquery-performance-at-scale/
* theopolis has been awarded as one of the top contributors in
  open-source security:
  https://twitter.com/TheZachW/status/1229149845593587712 Gratz!
* More BPF work happening, tracing DNS resolution requests from
  processes:
