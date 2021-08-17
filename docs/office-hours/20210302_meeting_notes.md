# osquery office hours 2021-03-02

YouTube Link: https://youtu.be/KB92ElIL77c

## Announcements and Highlights since the last meeting

## Any Questions / Issues / PRs people want to discuss?

The intent of this section is to provide a clear time for community members to bring up _anything_.

Broad questions? Bugs? Deployment questions? Blocked PRs?

### An unhandled exception when enabling BPF on the wrong kernel version

Zach thinks that maybe there was an unhandled exception arouind BPF kernel version. This is partly because there are some syscalls that are unhandled. However, this should have been fixed as of 4.5.1 or 4.6.0. Probably all exceptions should now be handled. 

This _might_ have manifested as a `TOB error` or `TOB String error`

Need an issue for more digging

### local results directory and osquery startup

If osquery is configured with a local file based log path, and it doesn't exist, should the log directory be created? 

Both ways can cause issues -- One can cause inadvertent disk fills. The other can cause osquery crash loops.

The general sentiment of the room is in favor of creating the directory. Not sure anyone is volunteering to PR it, but there's general support.

Similarly, what about the flag file? Should osquery start without it? Sentiment here is that if the flag file is specified, then osquery should not start. Some discussion about how to do this in the MSI -- we want to avoid having MSI installs overwrite configs. What do we want the out of the box config to be? Some discussion of reviewing all the platform installers and the default state.

## Agenda down here

## Host enrollment procedure

What happens when the enrollment procedure fails? Based on the failure type, if forwarded by the TLS server, we could change how osquery retries

1. If the failure is due to duplicated IDs, does it make sense to keep trying?
2. Would it make sense to force a different host identifier in this case?
3. Does multiple hosts attempting to contact the server all with the same identifier, cause an effective DoS?
4. Is 'hostname' the best choice for the default identifier, if it is more prone to accidental change and collisions than `instance`?

At least some observations here, is that we see the same identifier over multiple machines. Some possible causes:
* Cloning a machine with an osquery install can create the same sort of uuid. 
* Same hostname

Thinking about it:
* This is really the job of the TLS server
* Maybe osquery just provided everything, and we can remove the configuration flag
* Some of this can probably go into the additional enrollment information blob?

Usecases that might not be obvious:
* What if you want to preserve the continuty? Either because of upgrades, or hardware replacement
* Motherboards don't always have real uuids
* Remember though, subsequent communication happens on the node-key

General "plan":
1. Anyone with problems today should probably be using `instance id`
2. It's probably worth creating a mechanism to have a TLS server trigger regeneration of the instance id
3. We should make sure `instance id` and `uuid` are both in the enrollment information (This is an update to `kEnrollHostDetails`, it looks like it is)
4. Eventually we should remove the enrollment identifier flag

## 4.7.0 Release

https://github.com/osquery/osquery/milestone/53

Two things remaining:
* [chrome extensions refactor](https://github.com/osquery/osquery/pull/6780)
* [inode parsing causes crashes](https://github.com/osquery/osquery/issues/6977) - This should be `stoul` not `stoi`. This error is from 4.4.0 and we thinks this might have been fixed since)

## macOS Big Sur version/naming bug: fixed or not?

https://github.com/osquery/osquery/issues/6840

Maybe we can get consensus on whether this is resolved. ToB thinks if osquery is built with 10.12 SDK (which it should be, now), the problem should be gone.

Revisit this after 4.7.0. We can test the release

## Users want ARM packages

Directions for generating these on AWS apparently exist, and it would be nice if someone on the TSC besides Alessandro can attempt to follow them. Stress test our documentation, and create some ARM packages for 4.6. Then later for 4.7 too.

Yes they do. 

Alessandro needs to get AWS creds, so he can upload

## Augeas

seph dug at this recently. If folks are curious...

## Look at old PRs 

_(If there's time, we've been trying to re-visit old PRs)_

[Reverse Sorted List of PRs](https://github.com/osquery/osquery/pulls?q=is%3Apr+is%3Aopen+sort%3Acreated-asc)
