# osquery office hours 2022-03-29

YouTube Link: https://youtu.be/jmSryGNPGII

## Announcements and Highlights since the last meeting

## Any Questions / Issues / PRs people want to discuss?

The intent of this section is to provide a clear time for community members to bring up _anything_.

Broad questions? Bugs? Deployment questions? Blocked PRs?

## Agenda down here

## Windows code-signing certificate renewal

* What happens to osquery in deployment, if this expires? Everything signed is fine until 2031
* Time remaining to address this renewal? ~2 weeks
* Blockers on getting a certificate associated with the osquery foundation? We are not the Linux Foundation.
    * possibly, a publicly posted mailing address associated with our identity. We can put something on the osquery.io site?
    * there's a phone number, through Twilio. Seph can check on the status of this. Also where is that posted, is it in a TSC doc?
* Seph and Nick recommend trying to issue a certificate from DigiCert
* Are we ready to switch the certificate signing identity on our Windows users?
    * Only a problem for users who pinned the cert
    * We should get the new cert NOW and get past the SmartScreen phase where Microsoft doesn't trust the cert
    * Microsoft does not publish the requirements (time, number of installs) for when it will trust (prompt or not prompt)
    * For $500 of the osquery funding pot, we could renew Nick's cert and avoid rushing to a new certificate
* We could also get an EV certificate to avoid the Microsoft SmartScreen obstacle, but they deliver the key on a physical hardware token

## osquery 5.3 release

* Status of library dependency updates for 5.3
    * https://github.com/osquery/osquery/issues/7494
    * https://github.com/osquery/osquery/issues/7494#issuecomment-1071083041
        * Merged PR to remove libelfin https://github.com/osquery/osquery/pull/7524
        * Merged PR to update librpm https://github.com/osquery/osquery/pull/7529
        * We had discussed updating libudev but the CVEs are in a transient dependency, libsystemd, and the alerts are false positives

## ssdeep library is unmaintained

https://github.com/osquery/osquery/pull/7525

It looks like the [`ssdeep` library](https://github.com/ssdeep-project/ssdeep) is unmaintained. And we have reports of some crashes in it (See the upstream issues). There are also no tests here. Accordingly, it feels like a liability, so we think we should probably drop it.

This makes seph a little sad, the idea of fuzzy hashing is very powerful. But if it's not maintained, we should be making hard choices. 


## Communication of security advisory w.r.t. dependencies

* osquery probably needs a public advisory somewhere easily findable, with its exposure or non-exposure to known CVEs in dependencies. Basically the Google Doc that the TSC has now, but public. Putting it into the ASSURANCE.md doc for now seems advisable. Opened as https://github.com/osquery/osquery/issues/7538
* osquery probably needs a Software Bill of Materials (SBOM) that includes both direct and transient dependencies (not enough to point people to a CMakeLists directory)
* Follow-up on last office hours: did we evaluate off-the-shelf dependency monitoring solutions, before we roll our own?
    * Yes. Zach looked at Snyk and spoke to sales there -- they don't support the public reporting we would want.

A tactical scanning note:
* don't clone deps (as is done with a git recursive clone), let cmake do it

## Caching `users` and `groups` on windows

https://github.com/osquery/osquery/pull/7516

This implements a refreshed materialized view of users and groups. What do we think?

Zach is in favor. While this pattern might not make sense for everything, users and groups are a fundemental requirement of osquery.

Does it make sense to use sqlite's connect and create seperately, eg doing it the sqlite way? Not clear, this implementation is, perhaps, more memory efficient, since it's in binary. We're not totally sure how this would work.

## Node reenrollment and stale databases

https://github.com/osquery/osquery/issues/7522



## EndpointSecurity based FIM table (`es_process_file_events`) on macOS

(Will open a blueprint, but wanted to have some quick thoughts from others)

I am working on EndpointSecurity based FIM functionality. This will bring macOS closer/parity with Linux only `process_file_events` table.

However, while this is expected to be a bit of a firehose, the `NOTIFY_OPEN` event creates a lot of events. On my machine this `NOTIFY_OPEN` alone is pushing about ~1 million events a minute.

```
osquery> select * from osquery_events where name = 'es_process_file_events';
+------------------------+------------------+------------+---------------+---------+-----------+--------+
| name                   | publisher        | type       | subscriptions | events  | refreshes | active |
+------------------------+------------------+------------+---------------+---------+-----------+--------+
| es_process_file_events | endpointsecurity | subscriber | 1             | 7289971 | 0         | 1      |
+------------------------+------------------+------------+---------------+---------+-----------+--------+
```

This puts a decent load on CPU and memory, and also rocksdb grows significantly too. Watchdog would have a difficult time keeping up. 

Thoughts on:
* Not supporting `NOTIFY_OPEN` altogether
* Throwing it behind `--i_understand_the_risk_please_allow_open_events` flag 


**Implementation details**
* One publisher, multiple subscribers -- single `es_client`
* path muting (will affect other es based tables too)

If at all useful, this is how I implemented FIM in a Santa-like Swift program: https://github.com/trailofbits/sinter/blob/master/Sinter/components/EndpointSecurityClient/src/EndpointSecurityClient.swift#L89 - Alessandro

## Should `apt_sources` and `yum_sources` tables be Linux only?

https://github.com/osquery/osquery/pull/7537 and previous context re: `dpkg` https://github.com/osquery/osquery/issues/3323


## Look at old PRs 

_(If there's time, we've been trying to re-visit old PRs)_

[Reverse Sorted List of PRs](https://github.com/osquery/osquery/pulls?q=is%3Apr+is%3Aopen+sort%3Acreated-asc)
