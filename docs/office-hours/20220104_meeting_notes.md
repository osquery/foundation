# osquery office hours 2022-01-04

YouTube Link: https://www.youtube.com/watch?v=T_g1aiVrOoo

## Announcements and Highlights since the last meeting

- Merge of PR 7330 incorporating initial native build on Apple Silicon, thank you to everyone who helped

## Any Questions / Issues / PRs people want to discuss?

The intent of this section is to provide a clear time for community members to bring up _anything_.

Broad questions? Bugs? Deployment questions? Blocked PRs?

Please Review:
* Audit throttling PR needs review https://github.com/osquery/osquery/pull/7329
* Action for gating merges https://github.com/osquery/osquery/pull/7427 (Zach approved)

## Agenda down here

## 5.1.0

Is this stable? (Yes)

## 5.2.0

seph and Alessandro got builds running

beta builds should be good

Some of the library build flags changes. So it's worth doing testing

## Adding and removing TSC members. Active requirements


What do people think about adding minimum participation guidelines for membership to the charter? seph isn't in favor, thinks we're too informal. 

Do we want to remove inactive members? seph thinks there's one. Is it worth doing anything about it?

Some discussion about what the pros and cons of this. Do we have quorum issues?

Some discussion about whether to new people. Is it recognition for work? Vote for future confidence.

What's the goal of the TSC?
* Set vision for the project
* we have vendor representation -- consume osquery logs
* bpf stuff
* Bringing in what else?
  - mac support
  - containers


## Unintuitive Joins -- Revisiting the SID/GID on windows conversation

https://github.com/osquery/osquery/issues/7417

(We discussed this previously)

There's a common query that joins users to groups. (There's one in the FleetDM sets

But what is this getting, anyhow? On windows, there is no primary group. So this join may be a performance hit, for no benefit. Is there any point to the GID column on windows? We currently return the _first_ local group which is not, strictly speaking, its primary group at all. 

Takeaways:
* the primary group columns on windows is incorrect
* It could be set to the actual primary group, but this would be confusing, since that often domain users, which isn't always local
* It could be set to null
* The existence of this column with incorrect data, confuses people
* There are also a lot of recursion/inefficienies in the code

seph thinks there's a lot of uncontroversial work that could be done here.

There's a side discussion about just updating whatever part of FleetDM is hitting this.


## Performance metrics on `distributed` queries

Haven't opened a blueprint issue yet, but wanted some initial thoughts on adding `cpu_time/wall_time` like metrics to the distributed query results. This could be feature flagged. 

Remember that distributed queries happen in parallel, so process stats might be slightly off. Is there a way to handle this? How do we handle this on the existing scheduled queries? What about a mutex? We used to have one, but maybe we should bring it back. Arguments in favor of bringing back a mutex -- Limits CPU usage, and since we don't have thread differenciation anyhow. And practically speaking, distributed queries are serial, and scheduled ones are infrequent.

How would we return this?
* We could return it with the data, like we do the exit code?
* We could keep a short lived `distributed_queries` table
  - Better transparency 
  - in memory? life of pricess
  - Some kind of ring buffer
  - what about just using an event table

General support for:
1. Adding a mutex back
2. returning performane metrics with results, akin to the status code we already do
3. adding some kind of `distributed_queries` table, though purge semantics are unclear

## osquery exit code logging/documentation

We have exit code`78` defined as `exit_catastrophic`, but for other instances we just return `128` + the signal handler code. Any objections to adding more verbose logging when we hit this case? This can be a bit unintuitive while debugging, and added logging could help

Yes, general support for better logging

## Look at old PRs 

_(If there's time, we've been trying to re-visit old PRs)_

[Reverse Sorted List of PRs](https://github.com/osquery/osquery/pulls?q=is%3Apr+is%3Aopen+sort%3Acreated-asc)
