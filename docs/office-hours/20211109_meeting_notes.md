# osquery office hours 2021-11-09

YouTube Link: https://www.youtube.com/watch?v=VxnKkFpXlHE&t=8s

## Announcements and Highlights since the last meeting

## Any Questions / Issues / PRs people want to discuss?

The intent of this section is to provide a clear time for community members to bring up _anything_.

Broad questions? Bugs? Deployment questions? Blocked PRs?

## 5.0.2 Bugfix release?

There was a BPF crash for linux. There's a fix. Maybe we should hurry and get this out. Discussion about a 5.0.2 vs a 5.1.0

Discussion about how to coordinate this bugfix and also m1
* ToB wants m1 by end of year
* Can this bugfix wait for m1
* seph doesn't want to make a 5.0.2
* seph thinks we could do a 5.1 and 5.2 by year end

## Apple Silicon Support PR (feature)

Link: https://github.com/osquery/osquery/pull/7330

This is a big one. We need TSC to set aside some review time soon, it's almost ready.

How else can we help reviewers; what are the blockers for a merge? Some pointers have been documented by Mike here: https://github.com/osquery/osquery/pull/7330#issuecomment-964396679

### Updated CI workflow to cross-compile to M1. Tests are now run both on Catalina and Big Sur:
 - The main workers have been upgraded to Big Sur
 - An additional Catalina worker will take the pre-built tests and make sure they are working

### macOS artifacts have been updated to include:
 - Universal PKG and TGZ, unsigned
 - Universal package_data.tar.gz (consumed by osquery-packaging + osquery-codesign)
 
### Table fixes
 - **signature**: commit `tables: Add M1 support to the signature table` fixes on how signatures are validated on M1 binaries
 - **system_info**: commit `tables: Add M1 support to the system_info table` adds some additional code under `#ifdef __aarch64__` to support M1 systems
 - **suid_bin**: commit `tables: Remove deprecated no_push calls from suid_bin` is a small compilation fix that appeared when we updated Boost
 - **lldpd**: was not included in M1

### Libraries
 - Libraries used by macOS have been updated to the latest version available
 - Updated READMEs detailing how to regenerate the configuration files
 - libicu has been dropped, as it is no longer required/used
 - lldpd (and its table) was not included in M1

## Prevent the audit event system from using too much memory (performance improvements)

Link: https://github.com/osquery/osquery/pull/7329

## BPF support for CentOS/RHEL 7.6+ (feature)

Link: https://github.com/osquery/osquery/pull/7378

This PR adds support to our bpf_process_events/bpf_socket_events tables to the 3.10 kernel from Red Hat with backported BPF features.

## Look at old PRs 

_(If there's time, we've been trying to re-visit old PRs)_

[Reverse Sorted List of PRs](https://github.com/osquery/osquery/pulls?q=is%3Apr+is%3Aopen+sort%3Acreated-asc)
