# osquery office hours 2019-07-23

## Highlights since the last meeting

* Enable sanitizers (address, undefined) on Debug builds
* Fixing reported bugs first, but some of them require refactors
* Osquery version from git tag
* Certificate table on Windows was not working correctly!
* Buck has been added to the CI
* The MSI package could trigger crash loops
* More tests restored
* The battery table no longer returns rows on nil

## Action Items

### Pending Buck Work -- Feel free to help out

* Version dtring still hardvoded
* Restore missing tests ([#5629](https://github.com/osquery/osquery/pull/5629) restored to cmake)

### Contact LF with regards to the CLA -- seph

https://github.com/osquery/foundation/issues/2

### LF and github owners -- seph

### PRs for performance regression -- seph

https://github.com/osquery/osquery/issues/5668
https://github.com/osquery/osquery/pull/5667

### Apple package signing -- groob

https://github.com/osquery/foundation/issues/3

### Discussion / tickets about gsuite and money -- groob

https://github.com/osquery/foundation/issues/4

### v4 date -- Lauren to confirm with Mike about feasibility of end of august

## v4 stable release

Is there a date? Are we comfortable setting one?
* blocked on the dependency and toolchain work
* This is about the linux distros
* Can't do end of July
* Let's aim for august!

## Toolchain and dependency manager

Alessandro and Teddy are working on it

Some exciting work from Alessandro about his dependency manager
work. This is a layered approach, allowing things to be imported from
various places. Source, pre-built, s3 bucket, etc.

Once this is a bit further, they'll need help testing

## CLA

Yep. Still missing.

## Performance regression in the users table

* There are several interlinked issues
* Discussion about the performance regression in the users table
  - Conclusion is that we should back out PRxxxx and restore the old users table
  - Not yet sure if there’s a new table, or an is_hidden table.
* Seph will do this
* Discussion about a larger query planner and table caching issue
  - Seph will summarize on in a ticket
* Performance / benchmark tests might have caught this.
  - Stefano filed a bug.
  - What do we want to do for this? How much do we have for resources. Time? CPU?
  - we don’t think this is release blocking

## Code Signing

How do we take keys from facebook. Do we even need to?

Facebook has said they can keep signing, so this isn't a blocker.

Groob will investigate getting an apple shared cert.

## gsuite / groups / money

We don't have any email groups. Groob proposes we use gsuite, and that
we levy dues to cover people's memberships. This gets us not just
gsuite, but also a host of handy tools. (Several of us are familiar
with google cloud)

Relatedly, we don't have any tools in place to track money. Should we
use something like opencollective? A bunch of ledger files in git?
