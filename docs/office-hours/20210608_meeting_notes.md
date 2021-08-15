# osquery office hours 2021-06-08

YouTube Link: https://youtu.be/ySAQlQINSKI

## Announcements and Highlights since the last meeting

### Promoted aarch64 to stable on the website

We've considered aarch64 to be stable for awhile, and seph finally updated the website to reflect that. (This PR was a bit larger than expected)

## Any Questions / Issues / PRs people want to discuss?

### Blaed has a PR for Xprotect

https://github.com/osquery/osquery/pull/7138

## Should we make a 4.9.0 or keep holding for a 5.0.0

5.0.0 has some breaking changes:
* Paths are going to change from `facebook` to `osquery`
* New codesigning

Some discussion about signing keys. Seph did start on trying to an osquery windows code signing key. Nick is updating the build scripts. 

Interest in 4.9.0 stems from:
* We think people would like to see the dependency updates
* There are some solid bug fixes
* A desire to kick more tyres on various processes

Conclusion is that we should cut a 4.9 after Nick's and Blaed's PRs merge. Aiming for end of the week. 

## 5.0.0 app bundle packaging (macOS)

https://github.com/osquery/osquery-packaging/pull/3 Sharvil has tested this locally, but not end-to-end.

We should warn the larger community. Create issue, notify macadmins. (About both the app bundle and the larger path change). https://github.com/osquery/osquery/issues/7146 has been created, and slack has been pinged

## Upload to S3 and GitHub artifacts

ci/cd are publishing to a test bucket. When Teddy is happy with it, he'll cut over to the prod bucket. The test bucket is public. 

## Security Assurance Doc

https://github.com/osquery/osquery/pull/7048

* Should it be "Here's what is in Osquery and what isn't addressed" with the threat modeling we've thought about and what is/isn't covered? Or should it be a "Security Practices at Osquery" document?

## Look at old PRs 

_(If there's time, we've been trying to re-visit old PRs)_

[Reverse Sorted List of PRs](https://github.com/osquery/osquery/pulls?q=is%3Apr+is%3Aopen+sort%3Acreated-asc)
