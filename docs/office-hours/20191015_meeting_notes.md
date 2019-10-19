# osquery office hours 2019-10-15

Link to YouTube recording: https://youtu.be/4O_519SZQpQ

## Highlights since the last meeting

* Extensions support (PR #5851)
* Packages creation is now run on CI for testing purposes (PR #5881)
* Dockerfile to build and test osquery on Linux published (PR #5855)
* Python 2 scripts has been updated to Python 3 (PR #5846)
* Patched submodules are not in the source folder anymore, but in the build folder (PR #5850)

## Updates from Facebook

Groob asks if Ryan will be engaged again. We learn that this may not be the best meeting time for Ryan. Ted will try to find out.

Ted is merging bug fixes from GH to FB. It's been pretty smooth. One recent issue about http tests was created due to IPv6 testing. (https://github.com/osquery/osquery/issues/5883)
Need to revist `http_client` and changes in (PR #5606) to check IPv6 compatibility.

## Extensions support

* (PR #5851) is ready
* A small number of previously exported functions are no longer available as they were removed from the codebase. If they are no longer used internally, we should treat as dead code and we can remove them. In particular, this is in reference to two functions for serializing/deserializing json -- consensus is that they should be implemented in extensions if needed. (https://github.com/osquery/osquery/issues/5878)
* Boundaries of the SDK are not defined, resolving expectations can be a follow-up discussion.
* Need test for empty extension call failing back to looking up internal registries. Will depend on (PR #5836)

## Packaging

We found fixes for all CPack issues. The remaining issues are details and there are no blockers/limitations within CPack.

* These should be prioritized, who can help out?
* Ted proposes to use the manual upload/publishing of packages for version 4.1.0.
* Azure can create packages and upload to S3.
* Open question: How do we store private keys, Azure has an option secrets as variables.
* Code that Azure runs is indirectly user-controllable (land a sneaky PR) and could leak the keys.
* Azure keyvault has been explored, provides an HSM, and uses an API that most likely does not work with packaging tooling. These tools do not allow the flexibility we need, cannot provide a signed hash.
* Someone should take on exploring alternatives. One proposal is to separate CPack code into another repo that is frozen. Then only support TGZ (target for another release) building within osquery/osquery. Finally we create pipeline to ship the results of the osquery CPack build to a builder using the frozer repo, which also has the signing keys available.
* see also https://github.com/osquery/osquery/issues/5689

## OpenSSL and macOS Catalina

We need to switch Windows and macOS to use CMake/formula version. 

* Sharvil: Upgrading to 10.15 SDK has issues.
* Sharvil: On 10.15 there doesn't seem to be a way to have `/usr/include`.
* Alessandro: building OpenSSL with CMake is involved, following patch helps.

```cmake
execute_process(
  COMMAND /usr/bin/sed -i ".bak" "s+^CFLAGS=+CFLAGS=-isysroot /Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX.sdk +g" "${CMAKE_SOURCE_DIR}/Makefile"
  RESULT_VARIABLE result
  WORKING_DIRECTORY "${CMAKE_SOURCE_DIR}"
)
```

Documentation: https://wiki.openssl.org/index.php/Compilation_and_Installation#Modifying_Build_Settings

* Sharvil: we might need to install full Xcode (not just command line tools) to get the sysroot and full 10.15 SDK, we are unsure, we may have to install another pkg on Azure and update our docs.
* Lack of Catalina stability and access to machine could be blocker.
* Some osquery tables are slow on Catalina (Sharvil is investigating).

## Apple Developer Account

We need Apple account for osquery. These will be a required with notarizing macOS packages going forward. (https://github.com/osquery/foundation/issues/3)

(Sharvil); Able to notarize a simple hello world binary w/ his personal account. Unsure yet how to go about with pkgs. This needs investigating.

Seph via slack: The Linux foundation has said they don't have an ADC account. It's on us to run that riddle trail. And either pay, or get the tax id for LF

seph via PR review: seph has done a bunch of notarization around launcher. Once we have an ADC account, the commands are pretty straightforward. See https://github.com/osquery/osquery/issues/5894
## `auditd` vs `audisp`

Issue https://github.com/osquery/osquery/issues/5594

Nishant (Uptycs):
* replace `auditd` over a period of time to `auditsp`.
* `auditd` logs to disk, while `osqueryd` processes these records on the fly.
* Needs feature parity.
* Need to have a conversation: have osqueryd manage and ingest audit rules instead of `auditctl`.
* Open question how many people use `audisp`?

## Outreach

### Code Review

* Ted/Zach: it would be nice to have people help out with code reviews. 
* Github notifications are not helpful. It would be nice to have a Slack channel where people can request review.
* Zach: We now have `#code-review` channel in Slack. We should update PR template to mention a blurb about it.
* Ted: We should have more committers. General consensus is that we should reach out to involved community members with previous contributions.
* See also https://github.com/osquery/foundation/issues/36

### Hacktoberfest

* Nick: We should liberally label issues and PRs with `Hacktoberfest`.
* We need to do more to attract folks, and Hacktoberfest is a good way to get people involved. 
* We need to help newer contributors and might have to walk them through code-reviews and give them time to act before labeling PRs as `invalid`
* Perhaps have a "how to contribute to osquery" guide with expectations.

Note that the `#hacktoberfest` label is not needed for hacktoberfest to count the contribution. Contributors likely appreciate it, but hacktoberfest will recognize everything that isn't tagged `invalid`
## Misc

* C++17 support would be nice, especially interested in `string_view`. This seems to be doable on Windows and macOS.
* What is suspected broken from 3.3.x --> 4.0.x refactor, syslog, WEL. Are we making progress?
* Restoring `CLI_FLAG` semantics (PR #5882).
* syslog support in 4.x: (PR #5259)
