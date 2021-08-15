# osquery office hours 2020-11-24

YouTube Link: https://youtu.be/mP5_WU-IgDI

## Announcements and Highlights since the last meeting

* CI was broken, and then fixed. 
* ec2 tables now have windows support. 
* Extensions are somewhat less broken now. Examples are now part of the build. A deadlock got fixed
* rocksdb got updated. Earlier builds may be broken (We did some work and kept windows7 compatibility)

## Any Questions / Issues / PRs people want to discuss?

The intent of this section is to provide a clear time for community members to bring up _anything_.

Broad questions? Bugs? Deployment questions? Blocked PRs?

## Thrift

This is the Get Overlapped IO error

Alessandro did try upgrading to the latest Thrift, and didn't change much in the code (the issue still persisted).

Stefano is suggesting that silence this error by default, and add a flag to unsilence it. Discussion about whether or not we ever need to enable it. If it's always a false positive, why even have the option?

Some discussion about whether we can change the ping function to do the shutdown correctly. Alessandro says he tried, and found the generated code very strange. 

Long term, can we move away from Thrift? Our API is pretty stable, we may not even need these heavy RPC libraries. No real conclusion here. Just some chatter.

## Endpoint Security

Generall check-in on this state...

Sharvil has access to the developer account and working profiles. seph does not.

We need to fix the wifi test and update the build macOS.

seph should create:
  * Production distribution profiles
  * A developer profile for CI (CI also needs an account)

## Older Platforms -- Windows 7 & macOS 10.12

New RocksDBs stuff is dropping support for windows 7. We're good now, but maybe not next time.

Endpoint Security probably requires macOS 10.12.

Windows 7:
  * Kolide sees about 0.7%
  * Zach says he sees older windows stuff. 32bit windows. Etc

macOS 10.11:
  * El Capitan -- Last Release July 9, 2018
  * seph doesn't think this is much around anymore

We're thinking it might be reasonable to drop support. Especially for EoL operating systems. Somewhat, people can use older builds. And if someone needs an old OS, they can maybe build it themselves.

We might want to make it easier to disable parts of the code that won't build. This gives people an easier route to building for their weird old platforms. 

General feeling is that we _want_ to support everything. There's value in helping older releases. But sometimes we can't, and then we should upgrade.

For these platforms specificially:
* It sounds like we think we can keep windows7, so let's do it.
* It sounds like we mostly cannot keep macOS 10.11. And apple has a strong track record of getting people updated

## Next osquery release

Last release was october 5th. So end of November would be 2 months latter. But, we're hitting holidays.

Alessandro wants to make sure all the new libraries (systemd: dbus, expat, extended_attributes:libcap, BPF:ebpfpub) build and are solid on arm64.

Let's target the end of November for 4.6.0. If we hold, it that's fine.

## Look at old PRs 

_(If there's time, we've been trying to re-visit old PRs)_

[Reverse Sorted List of PRs](https://github.com/osquery/osquery/pulls?q=is%3Apr+is%3Aopen+sort%3Acreated-asc)
