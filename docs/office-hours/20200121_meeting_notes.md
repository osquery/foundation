# osquery office hours 2020-01-21

YouTube Link: https://www.youtube.com/watch?v=xO1o1N5o050

## Announcements and Highlights since the last meeting

- Thanks to Teddy that is still contributing with his awesomeness
- Thanks to farfella for all the work put into the windows
  UTF8<->UTF16 PR https://github.com/osquery/osquery/pull/6190


## Windows UTF8<->UTF16 PR #6190

Generally looks good, some c++ performance and optimization
discussion.

Discussion about how this could be implemented:

 * As a wrapper type (such as QString): on Windows, everything is
   utf16 and no conversion happens. If there are many instances where
   Win32 API etc are called, there is no performance loss. Downside:
   the conversion overhead is potentially moved to places where
   libraries that do not support utf16 are called.
 * As it is now (converting from/to utf8 and utf16 as required): If
   many conversions happen (i.e.: calling many Win32 APIs) the string
   conversion will add performance overhead. This is the most
   compatible way, and the current PR should improve performances
   compared to the current codecvt-based implementation.

We do not think this PR probably adds tech debt. so if we decide the
performance hit is too high, we can re-implement later.

farfella thinks that while this adds conversions over a straight
string, this is likely a performance win over the existing conversions
(as implemented by https://github.com/osquery/osquery/pull/6187)
because it uses optimized OS native functions.

The PR looks really good, and the only advice is to promote an
interface that discourages the usage of raw char pointers (like
.c_str()).

## A bunch from Teddy

Just before Teddy vanished, he opened a bunch of PRs. These are the
results of Teddy pushing facebook fixes externally. Nick is going to
try to pick them up and make sure they happen.

* #6183
* #6186
* #6189

## Windows datetime issues #5901

https://github.com/osquery/osquery/pull/5901 has been hanging
out. But, it's been stuck blocked on something about how buck file
includes work.

As an aside, there may no longer be a good reason to keep buck
around. Nick and Teddy are talking.

## eBPF

Alessandro is still working on BPF stuff. Initially planning to start
with a full integration, but he's shifted his goals a bit. Will likely
start with smaller tables. Possible candidates include:

* dns_events: DNS requests, attaching probes against
  getaddrinfo/gethostbyname
* bash_events: things happening inside bash / readline (interesting
  trigger for hosts that should be non-interactive)

Nick doesn't think Facebook is using the existing ev2-based ebpf
stuff.

Is there anyone in Facebook who might be good to review it? Alexander
would be ideal, but he's not really involved. We might be able to get
something from Ryan.


## 4.2.0 Release milestone

General themes:
* Bug fixes
* Windows improvements
* openssl versions

- Need to decide a due date
	- (Stefano) I would say end of February to stick with monthly
      releases, but I suspect we will have to drop some issues listed
- Make the releases more regular, especially normalize the time we
  wait between pre-release -> stable
	- (Stefano) If it's 1 week, it means we release 1 week before or after the end of the month?
	- (Stefano) We also push packages with an additional delay afaik,
	  how much is it?  If we want another week, will this go into the
	  next month or?  I think that for many (?) the release in the
	  packages repo is the official release.

Can we make a real release without Teddy? We can do a lot of the
release steps, but some of the signing and the linux repos are
harder. **But** Teddy will probably be back then.

Some discussion about moving to something closer to a CI/CD model. We
have continous builds. There are nightly/weekly packages. We encouage
people to use them. An official release becomes more of a rubber
stamp.

We should have a document that discusses loose guidelines about:
a. our release process
b. our timelines
c. commitments about how to do things

There is a commons problem here -- Are there people who can provide
testing for nightly or pre-release builds. Lots of people rely on a
stable OS query, but we don't think we a clear issue of people that
can run the betas. If we know what we want, we might be able to fund
raise for something here.


## TSC Update

- (Zach) Zach is speaking at osquery@scale and would like to give a
  quick (unofficial) update of what's going on with the TSC and
  osquery project. Do folks have items that should be included?
- (Alessandro & Stefano): Long term goals: improving container support
  in osquery through namespace APIs and eBPF tracers.

Nick has been talking with Ryan@Facebook. Ryan is happy to handoff his
role to Nick. Lots of things to talk through. Especially around roles,
and what being on the TSC means, and what any of it means. (This
probably isn't announcable at a con, but it's interesting news for the
TSC) https://github.com/osquery/foundation/issues/47

The big one is that we've started merging PRs, and there are stable
releases!

If people have other items, tell zwass on slack


## Build speed/caching Windows

(Stefano) Just a mention about improving build speed on Windows: I've
taken a look again at Mozilla sccache and fixed the sccache issue that
wouldn't make it work correctly with osquery. It is unclear if this
can be upstreamed or not.  The improvement is not comparable to ccache
on Linux because NTFS is still slow and linking seems to be the major
offender, but I've seen and improvement of 40% on the overall build
speed of RelWithDebInfo + Tests.

seph still needs to build buckets to hold artifacts, but this won't
directly help with the link step.

To improve linking, some big thoughts:
* change/reduce the AWS libraries
* have the tests roll into fewer executables
