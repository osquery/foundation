# osquery office hours 2021-01-05

YouTube Link: https://youtu.be/lOI0866PXpU

## Announcements and Highlights since the last meeting

## Any Questions / Issues / PRs people want to discuss?

### old ev2 blueprint closed

https://github.com/osquery/osquery/issues/5158

was closed as we think it's been addressed other ways

### old epbf blueprint closed

Nominate for closure: https://github.com/osquery/osquery/issues/5218 because we have retired this approach in Remove unused/experimental ebpf code in favor of the ebpfpub library approach and Alessandro's BPF-based socket and process events tables (#6571) in the 4.6.0 release.

### Zach is talking about epbd at osquery@scale

Go Zach! (and go Alessadro as well!)

## New apple log api (OSLog)

Garret is looking into the new unified log framework. So far, he only sees swift APIs. So, what's next?

It's not totally clear how this would work. Both the `cmake`, or the linking side. 

There's some indication that there's objective-c support for oslog. 

### Apple Silicon M1

seph has played around a little. Mostly gets lost in the dependencies. Nothing too hard, just updates. Rocksdb might be hard, since there's no release since the m1 code mergd. seph really ought be able to grant remote access to this machine.

ToB might have a machine they're spinning up as well.

And Uptycs is doing some m1 work as well.

## bpf -- do we have the right docs

Do we have good docs for people to easily pick up and start playing with bpf?

Our current bpf support is more about support _tables_ that use it. Not about running arbitrary bpf from the config files. (Though we couple implement that). So this implies we might want two different doc focuses:
1. For the osquery operator / security practitioner: What does bpf enable? Why is this cool? 
2. For the developer: How to develop a bfp based table

Probably missing is a doc about why one would chose `bpf` or `auditd` for process monitoring.
* bpf may use more cpu (there's a setting to allow more memory that might be more performant)
* bpf allows audit to stay running outside osquery
* Some of this additional CPU comes from resolving file descriptors
* It's not yet clear there are real world problems with the performance change
    - We can show differences using pathological profiling setups
    - Not yet clear there are real world impacts 

Zach will look into this a bit, and maybe make some recommendations or PRs

## 4.6 pre-release to release?

seph has not pushed 4.6.0 to kolide's beta hosts. General sentiment is that we should wait on that. 

We should remember to make arm64 binaries. Or use the ones from CI. These would need to be uploaded to the AWS bucket.

## Using CI/CD

Ben is working on blog post about CI/CD pipeline for building osquery configs.

* osquery's config check is good for very basic syntax checks. 
* But you might want to do runtime queries 

## Look at old PRs 

_(If there's time, we've been trying to re-visit old PRs)_

[Reverse Sorted List of PRs](https://github.com/osquery/osquery/pulls?q=is%3Apr+is%3Aopen+sort%3Acreated-asc)
