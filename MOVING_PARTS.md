# List of moving parts related to osquery

## Places to look for information

* Foundation Repository -- https://github.com/osquery/foundation
* osquery repository -- https://github.com/osquery/osquery
* 1Password Team Account -- https://osquery-foundation.1password.com

## Project Things

### `osquery.io` Domain

This is owned by the Linux Foundation

The NS is AWS and the zone is managed within Route53 by Facebook (@theopolis).

### osquery GSuite

This is available to the TSC and provides USERNAME@osquery.io inboxes.

The osquery@osquery.io inbox is a inbox-like-group that forwards to all TSC members.

### `osquery-foundation` 1Password account

This manages API keys, GPG keys, etc.

The users and owners include the osquery TSC.

Usage is free via a yearly check-in here: https://github.com/1Password/1password-teams-open-source



## Development Things

### `osquery` GitHub organization

Essentially this repo and all osquery foundation managed code.

The organization admins are the members of the TSC and the `linuxfoundation` management account.

### osquery CCLA and ICLA

This is managed within the Linux Foundation's EasyCLA tool.

Members of the Linux Foundation can help with changes. There is a installed GitHub app called `CommunityBridge: EasyCLA`.

### Azure CI

This is maintained by Trail of Bits.

It requires integration into the osquery GitHub org.

### `osquery-packages` S3

This is maintained by Facebook (@theopolis). The content is 100% public.

It contains dependencies for the build, in some cases pre-built dependencies.

It hosts the content for package (RPM, PKG, etc) releases as well as the content for YUM and APT repositories.

### PyPI `osquery-python` module

https://pypi.org/project/osquery/

The owners include the osquery TSC, managed through the PyPI website.

The code is managed in the [`osquery/osquery-python`](https://github.com/osquery/osquery-python) repo.

Updating requires bumping `osquery/__init__.py`'s `__version__` field. And optionally tagging a release on GitHub.

### ReadTheDocs

https://osquery.readthedocs.io/en/stable/

The admins include the osquery TSC, managed through the RTD website.

The content is hosted within the [`osquery/osquery`](https://github.com/osquery/osquery) repo, in the [`/docs`](https://github.com/osquery/osquery/tree/master/docs) folder.

Anyone submitting PRs to osquery can update the documentation.

### `@osqueryer` GitHub bot

### Coverity Static Analysis

We have a dashboard page setup at https://scan.coverity.com/projects/osquery 

Various members of the TSC are project owners. 

## Social & Media Things

### Slack

There is the Slack workspace, admins include the osquery TSC.

There is a auto-inviter (slackin) configured within a Droplet at https://osquery-slack.herokuapp.com


### `@osquery` Twitter account

This twitter account is configured to grant various TSC members
posting permissions in [TweetDeck](https://tweetdeck.twitter.com). You
should be able to login and use it.

The underlying credentials are in 1password.

### Reddit Admins

### Keybase User

## Money Things

### CommunityBridge Fundraising Platform

osquery exists on communitybridge's funding platform --
https://funding.communitybridge.org/projects/osquery In theory any
members of the TSC can file expense in that. seph (@directionless) is
probably the entry owner.

### Readthedocs Money
