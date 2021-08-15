# osquery office hours 2021-02-16

YouTube Link: https://youtu.be/Dzt5wGu-y6A

## Announcements and Highlights since the last meeting

## Any Questions / Issues / PRs people want to discuss?

The intent of this section is to provide a clear time for community members to bring up _anything_.

Broad questions? Bugs? Deployment questions? Blocked PRs?


## 4.7.0 ?

It's been about 2 months, there have been [88 commits](https://github.com/osquery/osquery/compare/4.6.0...master). 

Let's try to get a couple more PRs in:
* https://github.com/osquery/osquery/pull/6780
* https://github.com/osquery/osquery/pull/6927

Zach might review [shellbags](https://github.com/osquery/osquery/pull/6949)

Current thinking is we should use the existing flow for this. There's enough commits waiting, and try to get new certs and process for 4.8.0

## Alessandro's CI work

The packaging parts are working. Code signging is still ongoing

Still have to fill in more of the signing code. Needs the right sign tool. Nick uses digicert for his cert, we should aim to get an osquery one for the 5.0 release.

It turns out that to make debug packages we need cmake and cpack. This ends up needing to keep some versions in sync. 

## Where are we on the Azure -> GitHub transition

GitHub as CI seems to be working. 

Both Teddy and seph advocate we turn off Azure, because it's extra work and confusing.

Next Steps:
1. Make github actions a required check (done)
2. Remove the azure pipeline (PR'ed)
3. Remove azure from the required checks (done)
4. Improvements to github actions

## Discussion about logs

The logs could use a little more structure. Parsing these has a lot of string parsing. One concrete step would be to add the query name to the structured tags

Another issue is that the watchdog doesn't have any plugins. Which means logs from it don't get places. This is especially an issue on windows. Some discussion about how we might have the watcherdog get lots to a plugin. IPCs? Files? The rockdbs single filehandle is an issue.

If we wanted to have some worker/watchdog IPC, we want a really minimal cross platform IPC. Something that isn't going to complicated the watchdog. Boost has an IPC interface that might be good.

Another option would be to have the watchdog dump something into a file. Which a startup process could read. This can work for watchdog -> worker, but not a good fit for osquery heartbeats to the watchdog.

## Discussion about performance and profiling

Some side discussion of osquery profiling (Split off from discussion around tracepoints in watchdog monitoring)

There are a couple classes of perforemance issues

1. Pessimal join -- if you gets your joins wrong, you can call an table once per row
2. Tables that are mis-advertising indexes
3. Slow OS apis
4. Things like enumerating the filesystem

Some of these problems come from the sqlite core assumption that data is on disk. 

One thing that might help is just knowing how many times a generate function is called per queuery.

The `--planner` option can help explain some things. 

osquery has a queuery cache that is supposed to help. seph says sometimes it doesn't seem to work right. 


## Look at old PRs 

_(If there's time, we've been trying to re-visit old PRs)_

[Reverse Sorted List of PRs](https://github.com/osquery/osquery/pulls?q=is%3Apr+is%3Aopen+sort%3Acreated-asc)
