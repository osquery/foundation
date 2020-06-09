# osquery office hours 2019-06-09

YouTube Link: https://youtu.be/u7_wiG9nxlE

## Announcements and Highlights since the last meeting

## Any Questions / Issues / PRs people want to discuss?

### Integrating osquery with Secureworks endpoint agent (Alex)

Welcome back Alex!

Does the fork into a container work from `osqueryi`? Yes. Forking
should work. The implementation is not a general "run this qury in a
container", this is because tables need some changes, and cannot use
any kind of global state. (`glog` for example)

### Performance issues with `users` table on Windows:

https://github.com/osquery/osquery/issues/6411

If there's a domain controller, this table can cause network hits. At
least now, we think this is probably correct. These users are users.

The other issue here is that we flag the `uid` column as indexed. It's
not on Windows. We might be able to work around this by splitting the
schema definition by platform, and using extend to flag it as indexed
on the other platforms.

## 4.4.0 Release

https://github.com/osquery/osquery/milestone/49

Currently aiming for Friday.

We're mostly waiting on #6280. It needs less than a day of work. ToB
is looking for someone to wrap it up.

## EndpointSecurity PoC

https://github.com/osquery/osquery/pull/6467

Update on EndpointSecurity backed `process_events` table

* Still a WIP
* now unified table name `process_events` -- would like an opinion on
  the implementation (there are a few ways around it)
* Several possible approaches

## Docker Hub

Zach is getting ready to publish osquery docker images. Yay!

Anyone know who owns the osquery docker hub account? It doesn't seem
to be anyone currently in the room.

Meanwhile, we have `osqueryfoundation`.

## Releases from CI

Within the next 30 days, Teddy's code signging cert will expire. So,
this makes it a good oppertunity to revisit code signging certs and
automation.

Open Questions:
* Where to artifacts go?
    * seph thinks s3, probably
* How do we move keying material around?
    * Where is it stored?
    * How is it moved into the Azure pipeline?
* Are we using HSMs?
    * seph thinks not
* Seperate Repo
    * We should start simple, we can defer this
* Probably also need a osquery azure account
    * seph should probably
* Will changing the codesigning key impact anyone?
    * https://github.com/osquery/foundation/issues/61 has been created
      and macadmins slack has been pointed at it.

## Let's look at some PRs

Some suggestion to close PRs that haven't been touched in ages. We
made some progress.
