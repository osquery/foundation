# osquery office hours 2021-03-29

YouTube Link: https://youtu.be/ZoNKzGPx9Bc

## Announcements and Highlights since the last meeting

## Any Questions / Issues / PRs people want to discuss?

The intent of this section is to provide a clear time for community members to bring up _anything_.

Broad questions? Bugs? Deployment questions? Blocked PRs?

### osquery TLS logger retrying repeatedly

Dan was asking about https://github.com/osquery/osquery/issues/7021

Using osquery and fleet. 4k hosts, mostly laptops. A couple users are going over bandwidth limits. This is odd, since they're doing very little with the queries. Coming into fleet they see a few TB, which feels high to them. Doing packet captures, they see a lot of duplicate packets. (Also seeing corp firewalls overloaded)

He's been diving in, and thinks some clients get 400 error back from the log endpoints. 

One thing to note, is that if you have connectivity logs, osquery might log those errors. Which are then buffered. And you end up with an increasing thundering herd problem.

General theory is that this 400 is coming from nginx. Dan thinks this because of packet captures. Fleet probablty does not return 400 errors, which lends credibility.

Ben suggests:

* https://github.com/CptOfEvilMinions/FleetDM-Automation/blob/main/conf/ansible/nginx/nginx_fleetdm.conf
* https://defensivedepth.com/2020/04/02/kolide-fleet-breaking-out-the-osquery-api-web-ui/


## openssl vulnerability

https://www.openssl.org/news/secadv/20210325.txt

2 vulnerabilies
* TLS renegotiaion on _servers_
* certificate strict checking

We do not believe we're effected. osquery does not bind to anything as a tls server.


## 4.7.0 release

https://github.com/osquery/osquery/issues/7003

Seems stable to Kolide. And the handful of people using it.

Sounds like it's stable. 

## Arm64 Update

Folks are intersted in the current state of arm64. A recap from seph.

Background -- osquery compiles and works, but has been left as unofficial beta pending some kind of CI integration. osquery uses GitHub actions for CI, which does not have vendor provided arm64 support. But _does_ provide some options to self-host runners. 

Unfortunately, GitHub Runners do not have strong security controls for running essentially untrusted code in a PR. GitHub calls this out in their documentation. We are currently experimenting with two approaches to solving this.

One approach is to have the GitHub workflow start a runner, do the work, and then stop the runner. Because GitHub needs to start an AWS runner, GitHub much have a suitable AWS secret. **But** GitHub (quite reasonably) won't share this secret with forked repositories. Thus, for this approach to be secure, it can only run on branches in the orignal organization. For our purposes, this means the CI will run after merges to the main branch, and, on any feature branches in the osquery org. While not perfect, this is a pretty good stop gap. This work is in https://github.com/osquery/osquery/pull/7014 and is likely to merge soon.

The other approach is to setup ec2 nodes as runners. There are a couple stumbling blocks here. The runners would, ideally, only run a single job before being rebuilt. This does not appear to be how the runner code is designed to work. But, the bigger issue is the security conext. The runner exposes a variety of runtime credentials to the job it's running. So we need to remove or limit those credentials. seph is working on this with a couple of AWS engineers (Thank you amazon!) and a jumping off point for this work is https://github.com/osquery/infrastructure/pull/7

## Packaging Logic (ToB)

https://github.com/osquery/osquery/pull/6921

Merging this would unblock additional work to add EndpointSecurity-based tables (macOS) and ARM CI (Linux, eventually macOS).

seph thinks it's reasonable to merge and iterate. PR has approvals from both seph and Teddy

## teddy's todo

Just a quick heads up:
- Looking at the watchdog code closely. One goal is to simplify some internals. If there are known-problems (or things you want triaged), point them my way by linking them here:
    - (Zach) Does the watchdog handle distributed queries? I think it does not. Can it?
- Similarly... I have a proof-of-concept IPC for the watchdog, ETA a few weeks and a few PRs of simplifying watchdog code before this is workable.
- Experimenting with reducing binary size of tests on Windows (ongoing after refactoring carver). SQL and virtual tables are my next target for cleanup. Hopefully more code organization and 'unwinding' of tightly coupled features means less linking requirements.


## Discussion about tables and builds

Several times in the past we've talked about building osquery with fewer tables. There are a couple vendors that seem to do this. (cisco ships an osquery without the carver. Uptycs's basequery). Can we bring this back into upstream?

We used to support this, pre-experimental branch

Discussion about how to do it well. Tables would need to have a more clear list dependencies so cmake could do that resolving. 

## Look at old PRs 

* Basic logrotate: https://github.com/osquery/osquery/pull/7015 looking to land and iterate with feedback from those asking.
* https://github.com/osquery/osquery/pull/6254 -- dns events are going to be incomplete, is this still valuable? Conclusion -- yes
* https://github.com/osquery/osquery/pull/7006 -- secomp seems valuable. People are looking at it. 
