# osquery office hours 2019-11-25

YouTube Link:

## Announcements and Highlights since the last meeting

* 4.1.1 has been in pre-releases
* Fuzzing is live. 
  - You can go to
    https://github.com/google/oss-fuzz/blob/master/projects/osquery/project.yaml
    and add yourself to be notified of discovered crashes.
  - There are some crashes being found in the config parser

## Any PRs people want to discuss? Anyone here for help?

There's a question about how to use the `socket_events`
table. `local_port` and `local_address` are zero. Though this is
confusing, it is also what the underlying audit system emits. You need
to track the calls, and the FDs that are used.

## 4.1.1

https://github.com/osquery/osquery/milestone/45

- Release to packages (complete)
- Release to website (in process) https://github.com/osquery/osquery-site/pull/156
- Release to repos (??)

seph will verify hashes in
https://github.com/osquery/osquery-site/pull/156 and merge after this
meeting

We've observed people will automatically pull from the stable
repos. So we think there should be a 1-2 week delay before we push to
the stable packaging channels.

## 4.2.0

Discussion about whether we should start merging for this. We're not
in the stable repos yet, so we might see an issue. But we've been
released a bit, and we think we should be fine.

What are the 4.2.0 release focuses. Brainstorming:
* python test framework
* dependancy caching (especially for windows)
* more automated build/packaging tooling
* feature parity between platforms (kafka on windows comes up)

Discussion about how these are primarily infrastructure oriented. They
make our releases easier. _But_ this is ultimately to support the
table and feature work that people do.

Are there things users have brought up?
* windows event log table (Search windows events)
* parity gap with windows
  - windows tables often need love, and optimization
  - batching writes to the database in evented tables
  - There's good-first-issue work around doing column optimizations (plumbing work)
  - The windows_event table's `data` column is parsed by some hand
    tooled xml. Moving to ramidxml would be a big
    win. https://github.com/osquery/osquery/issues/6083


I think we're settling on a windows focus

## Automatic packages (through CI)

We really want this not to be a manual thing that Teddy does. What are
the current blockers? Anything side from doing the work? Any decision
points to be discussed?


CI can already build the packages, but they're not uploaded
anywhere. Whether it's worth doing this without signing id debatable.

We've discussed a split build / package flow, we continue to think
this makes sense.

* the gpg key is in 1password
* The current apple key is Teddy's, and we do not plan on using that. 
* Nick's personal cert signs the windows side. Changing keys will require the reputation reset
* The s3 bucket is currently in facebook space. 

seph, Stefano, Teddy, and Nick should try to split this up a bit.


## Results from syncing Facebook changes

Several changes that need investigation (follow-up). This means Teddy
will review the Facebook change, determine if it is applicable for
GitHub, if so make the patch and create a PR.

Based on studying the changes since April, here are the potentials:
- Adding more tests for the processes mini-lib.
- Clean up time-based test checking in unit tests to reduce flakyness (broad but a minor amount of edits).
- Improve IPv6 tests.
- Add unit tests for isPlatform and hostnames.
- Add unit tests for hashing mini-lib.
- Add unit tests for process-opts mini-lib.
- Add logging for shutdown requests.
- Append the configuration source-name to scheduled query names (API break).
- Port OpenSSL to 1.1.1.
- Move enroll TLS plugin into /plugins.
- Add checks for type conversions in Windows processes tables.
- Use TLS config source-name instead of hardcoded "tls_config".
- Backport patch for librpm to mitigate ASAN error.
- Shorten thread names for all platforms.
- Improve versionAtLeast and the documentation.
- Improve file read operations.
- Improve file carving tests on Windows.
- Fix rare memory leak in librpm.
- Fix IPv6 search domain parsing in dns_resolvers.
- Improve WMI class (error handling, testability).
- Improve Windows interface_details error handling.
- Improve TLS file/flags error handling.

Teddy says he's going to try to work through this, and we'll probably see some PRs

Thank you Teddy for bridging these code repos. The better the tests
are, the easier this work
is. https://github.com/osquery/osquery/issues/5990

## Anything worth recognizing (tweeting/blogging)

* 4.1.1 release?
* Open invitation for new issues / features
* Encourage people to upgrade
  - talking up the new features, what to expect.
  - 100s of bug fixes
  - Close with "what else would you like to see"

## Restoring FreeBSD

Azure doesn't support FreeBSD, so we don't currently have it.

It hasn't been looked at very closely since the 4.x branches.

The may be issues with the dependancies.

There are some FreeBSD users inside Facebook, but it doesn't sound
well maintained.

Is there a FreeBSD expert in the community?

## Python 3.5

https://github.com/osquery/osquery/issues/6079

https://github.com/osquery/osquery/pull/6081 is open to fix it

## Foundation / Org Update

- Seph talked to the LF. We now know our Tax ID, LLC name, etc.
- Notable is that we're a 501 c6
- Slack may not recognize c6 as a non-profit, so for now our slack
  plan is going to continue to be paid for by Facebook.  Expense is
  ~$1000/month. They are aware. Not "at risk" for now.
- GitHub discussion is ongoing
- Apple code signging certs are ongoing

## Arch Linux

- Arch like using system libraries. This requires an additional dependency layer
- This patch is lightweight enough to fit inside a single directory under libraries/cmake/system
- The only supported binary is the one built with the source layer
- The osquery toolchain is optimized for static cross linking. This
  work is contrary to supporting system libraries. libunwind is an
  example of this
- Building with gcc creates a lot of warnings. Should we look at those
- We have a smarttools fork
  - there's a huge divergence with upstream
  - Maybe we can upstream our changes
- We store generated code (thrift, flex, bison)
  - As is, this can be regenerated, but that we want to keep checking it in

There's some work happening in https://github.com/osquery/osquery/compare/master...anatol:archlinux

Next Steps:
- update dpkg (small, seems safe with testing)
- update thrift (coordination point with SDKs. Seems scary)
- Linking issues: https://gist.githubusercontent.com/anatol/7363919b4a17b3ba75aec539f219b92a/raw/085c87a799381f3e1ff6b1ccbe5b23d3ea06fd1b/instructions.txt
- There's some concern about the library version skew between what's
  in current arch vs centos6. But we'll just have to test as we go.
