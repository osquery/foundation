# osquery office hours 2021-07-06

YouTube Link: https://www.youtube.com/watch?v=D9Ls6ZQHZSg

## Announcements and Highlights since the last meeting

### 4.9.0 is stable in github

I don't know if we've regenerated the rpm/deb repos to reflect this. 

## Any Questions / Issues / PRs people want to discuss?

The intent of this section is to provide a clear time for community members to bring up _anything_.

Broad questions? Bugs? Deployment questions? Blocked PRs?

## Privacy vs Forensics

There's been some slack discussion about where the line between privacy and utility lie. For example, there's been requests for Chrome History Items, and Arbitary File Reads tables. The generally consensus of the TSC ... FIXME

A bit of discussion about ATC tables.
* They can read Chrome History. Would we consider blocking ATC? Or adding a filter layer to block History? 
* Alessandro says it feels off to block a table, but provide a backdoor. 
* Conversely, we do have a clear intent around what things we parse/support, vs something we bake in. 
* There's an _intent_ difference.
* Malicious admins can likely push non-osquery malicious software
* The idea of forensic value : privacy cost
* Ideas of transparency to the user. How do admins convey what data _could_ be accessed and what data _has_ been accessed. 
* No consensus

It's somewhat tribal knowledge, and a bit of "I know it when I see", but some general goals:
* Browser history
* email
* messages caches
* contact information
* Private Key Data


Some discussion about whether having a hooks to cpack to let people embed extensions? Does tooling help?

FleetDM's orbit is designed to be an easy drop in for osquery, and Zach says he's working on making orbit support adding extensions. 


## Windows NTFS FIM Errors

Sebastiaan on Slack was running into a bunch of windows FIM issues. 

This looks like a stream of errors like `Failed to remap the rename records`

Maybe related to https://github.com/osquery/osquery/issues/5848


## macOS app bundle PRs

Mostly ready to go, couple of things came up from the review:

Consensus on location? `/opt/osquery.app` or `/opt/osquery/???/osquery.app`? 

Symlinks for `osqueryi` and `osqueryctl` in `/usr/local/bin`? It seems we don't want to have symlinks? Teddy mentioned adding a path in `/etc/paths.d`?

seph strongly thinks posix machines should use `/opt/osquery` as the install path. And the app bundle should be somewhere under that. 


## Windows certs for 5.0.0?

seph is still fighting with CAs


## Look at old PRs 

_(If there's time, we've been trying to re-visit old PRs)_

[Reverse Sorted List of PRs](https://github.com/osquery/osquery/pulls?q=is%3Apr+is%3Aopen+sort%3Acreated-asc)
