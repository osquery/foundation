# osquery office hours 2022-01-18

YouTube Link: https://www.youtube.com/watch?v=0eZNPoBY-uU

## Announcements and Highlights since the last meeting
* 5.1.0 went to stable
* 5.2.0 is out

## Any Questions / Issues / PRs people want to discuss?

Some slack question about the NTFS events table. Short discussion about how this might be. Whether it's a real error, or something overly noisy in the logs. We do have several issues #5848, #6992, #7195. Some of this is due to a windows10 API choice we have. 

## Agenda down here

## 5.2.0 Fixes, do we want a 5.2.1?

Yes, there's been some testing.

The library changes introduced a yara crash. We fixed it. https://github.com/osquery/osquery/pull/7439

There are other fixes in flight, but this is the only 5.2 introduced bug

We should cut a release today

## PR 33 (osquery-codesign) - Update codesign workflow, implement M1 support

This PR is required to sign new 5.2.0 binaries, and would help with testing EndpointSecurity on macOS

seph will review


## PR 7446 - bpf: Improve socket event handling

https://github.com/osquery/osquery/pull/7446

This PR fixes a bug and improves protocol auto-detection accuracy when handling BPF socket events.

Protocol auto detection is performed when:
1. A socket() syscall was either lost or called with no domain specified (AF_UNSPEC)
2. A sockaddr structure with family set to AF_UNSPEC is handled for that socket in response to one of the following events: accept, listen, bind

## CentOS 6 Support

https://github.com/osquery/osquery/issues/7445

We had some conversation about centos 6 support. There's broad developer interest in dropping support for centos6. 

But there is some interest from vendors in keeping that support. (See ticket) Discussion about what we can ask of these vendors. 

There are some lurking kernel headers / library issues.

Third party libraries may call things in the OS that changed. It becomes harder and harder for us to support things as the age changes.

At some point, we expect llvm to stop supporting a libc that old.

Trying to estimate what developer time might look like to fund this. (It's too unknown to ballpark the effort today. Probably not small)

What would it be like if we shipped an osquery where some tables just don't work on an older os? How would this be different from today, where we don't work on some newer OSes (librpm, iptables)


## PR 7436 output_size in osquery_schedule

https://github.com/osquery/osquery/pull/7436

I think this just needs one final pass, and can we add it to next milestone?

## wifi_networks table tech debt (macOS)

I am working on fixing the table for Big Sur and Monterey (apple moved the plist to binary and base64 encoded which is a bit of a pain), but I would like to drop support for parsing OSX 10.9 era parsing

General sentiment is that it's better to support the modern versions than the old ones. We're not _desupporting_ the old platforms, but we are updating this very specific table. 

## PR 7370 - Implement additional events in bpf_process_events

https://github.com/osquery/osquery/pull/7370

Opened some time ago, but recently updated with additional new event types:

 - cap_capable (old)
 - ptrace (new)
 - init_module (also enables finit_module) (new)
 - ioctl (new)
 - delete_module (new)

PR needs review. Note that this has a breaking change to the BPF schema.

seph thinks that this table is still very young, and likely not widely used. So if Alessandro thinks it merits a change, it probably does. Zach concurs that it is a young table, and better to change it.



## Look at old PRs 

_(If there's time, we've been trying to re-visit old PRs)_

[Reverse Sorted List of PRs](https://github.com/osquery/osquery/pulls?q=is%3Apr+is%3Aopen+sort%3Acreated-asc)
