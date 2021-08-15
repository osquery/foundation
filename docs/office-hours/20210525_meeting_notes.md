# osquery office hours 2021-05-25

YouTube Link: https://www.youtube.com/watch?v=0zpzWLbLQ10

## Announcements and Highlights since the last meeting

## Any Questions / Issues / PRs people want to discuss?

The intent of this section is to provide a clear time for community members to bring up _anything_.

Broad questions? Bugs? Deployment questions? Blocked PRs?

## Libraries with Vulnerabilities

It's unclear to us whether these are exploitable, or what the severity is. We're not sure if we should make a point release for these or not. We'll leave this as TBD based on any severity findings. 

### Yara Vulnerability

* https://github.com/osquery/osquery/issues/7123
* https://nvd.nist.gov/vuln/detail/CVE-2021-3402

> An integer overflow and several buffer overflow reads in libyara/modules/macho/macho.c in YARA v4.0.3 and earlier could allow an attacker to either cause denial of service or information disclosure via a malicious Mach-O file. Affects all versions before libyara 4.0.4

We think this would allow an unprivledged local user to leave a malicious macho binary around. Which osquery might then scan, and cause the malicious code to run in the osquery context. Eg: This is akin to a local privilege escalation

### liblz inside libkafka

https://github.com/osquery/osquery/issues/7097

We think this is a bug in the compression routine. Which, while theoretically exploitable, sounds hard to exploit.

### libmagic

https://github.com/osquery/osquery/issues/7096

## Osquery 5.0

Moving along. Shavil, seph, Teddy, and Nick have all been pushing code into the `osquery-codesigning` repo. 

### Chocolatey

Some discussion about whether the chocolatey package should just grab the MSI and then install it. This was suggested by some windows people on slack. An advantage here would be unifying package logic. 

seph is sketical, since there's not a lot of logic.

Stefano points out we _do_ have some ACL resetting logic around the MSI, and it was breaking in the chocolatey packages. And in general our manual ACL management has been a lot of trouble. 

We think this merits some investigation. 

### Windows code signing

Ideally we'd swap the code signing cert for windows away from Nick. The sense of the room is that we should get something from one of the vendors we're familiar with. 

seph will try to procure a comodossl/sectigo cert.

## Look at old PRs 

_(If there's time, we've been trying to re-visit old PRs)_

[Reverse Sorted List of PRs](https://github.com/osquery/osquery/pulls?q=is%3Apr+is%3Aopen+sort%3Acreated-asc)
