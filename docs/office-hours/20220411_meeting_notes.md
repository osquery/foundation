# osquery office hours 2022-04-11

YouTube Link: https://youtu.be/0nkVzp0GEkQ

## Announcements and Highlights since the last meeting
* 5.2.3 Was cut
* Sharvil is elected to the TSC
* seph is now the TSC Chairman

## Any Questions / Issues / PRs people want to discuss?

The intent of this section is to provide a clear time for community members to bring up _anything_.

Broad questions? Bugs? Deployment questions? Blocked PRs?

## Agenda down here

## Releases!

We cut a 5.2.3 that has cherry-picked updates related to updating dependencies.

We expect 5.3.0 soon.

The 5.2.3 linux is using the system zlib libary, there's a PR to fix this. 

## Extensions and Shutdown

There are some questions about how shutdown is supposed to work. 

There appears to be a shutdown message on the thrift socket. But if the watchdog kills it, there might not be a message sent. And there's another axis of whether or not osquery is managing the extension, or if it's started externally. And then there's the extension watchdog

## Look at old PRs 

_(If there's time, we've been trying to re-visit old PRs)_

[Reverse Sorted List of PRs](https://github.com/osquery/osquery/pulls?q=is%3Apr+is%3Aopen+sort%3Acreated-asc)

We spent a little bit of time talking about PRs. It would be nice to get a bunch of these into 5.3