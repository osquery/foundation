# osquery office hours 2021-03-16

YouTube Link: https://youtu.be/Uy8LlCwQxbg

## Announcements and Highlights since the last meeting

* 4.7.0 is cut

## Any Questions / Issues / PRs people want to discuss?

The intent of this section is to provide a clear time for community members to bring up _anything_.

Broad questions? Bugs? Deployment questions? Blocked PRs?

## 4.7.0 Release

We're waiting for a new windows MSI, and then we can upload to the website. (Should be done today)

seph and Alessandro will sync on AWS for the arm64 issues

## Run Queries at Load (zach)

Sometimes users want to run scheduled queries when they are first loaded, and then again on the specified interval. For example on infra that is to be short lived, it is desirable for a query to run at load or else it might never run.

Do I remember there's been some discussion on this recently? What do folks think? (People do not remember this)

Kolide Launcher had an implementation of this, but it was out of band and a bit flakey

seph thinks this a great idea, Kolide does something like this using live queries. But having it in the native osquery would be much simpler. 

Another use case is for ephemeral machines. If you have machines that only run an hour or two, but need to run at least once, having an run-early setting would simply their management

Have to beware about the watchdog. You might want to delay starting the watchdog until after ther initial runs. This brings back the watchdog API conversation

Related, but not the same, is the idea of a per-query epoch. And should an epoch increment trigger this?

Zach will open a ticket to discuss

## Turning off 'build tests' in dependencies (mike)

https://github.com/osquery/osquery/issues/6990

We apparently build dependencies with tests, that we don't run on every build, and we could be saving a lot of time/space on the compiles. Should we pass the right flag to each of them to disable tests, and agree only to build/run these tests when we update the dependencies?

There are times we _do_ want to run the tests. So we should maybe add an option. Either disable the tests, or actually run them.

## Windows Thrift errors (from seph)

https://github.com/osquery/osquery/issues/6991

Maybe, if we have time?

Did you observe what kind of overhead the extension has when it is killed (--mike)

Seph will create a dummy extension that experiments with the extensions watchdog. Will read the code, and one outcome might be better documentation of the extension watchdog. Other outcomes listed on the issue: better/cleaner extension shutdown and restart, etc.

## The state of ARM AArch64 support

We could use help / contributions of effort from the interested third parties (there are several). Running our own ARM workers has required some non-trivial configuration/exploration. 

We've explored running CI through Amazon Codebuild (their CI), and that seems like it will work, but we're hesitant to have to support two CI systems. 

Second option is to run the GitHub runner hosted on AWS. We're progressing slowly on this as time allows. Currently figuring out how credentials can be protected/secured, making them impossible to be leaked during the running of the build.

There is a PR for generating Docker containers. https://github.com/osquery/osquery-toolchain/pull/23

## Look at old PRs 

_(If there's time, we've been trying to re-visit old PRs)_

[Reverse Sorted List of PRs](https://github.com/osquery/osquery/pulls?q=is%3Apr+is%3Aopen+sort%3Acreated-asc)
