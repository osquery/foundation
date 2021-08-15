# osquery Office Hours 2020-03-17

YouTube Link: https://youtu.be/4wfBVkcpCGU

## Highlights since the last meeting

## Any PRs (or issues) people want to discuss

## aarch64 support

There are a couple of pull requests for this.

The toolchain one --
https://github.com/osquery/osquery-toolchain/pull/18 Alessandro was
able to test this in an emulator, and it's been merged. It will
eventually have an artifact that gets hosted with the other toolchain
artifacts.

We need to spin up a aws instance to run build and test the osquery
PR. (https://github.com/osquery/osquery/pull/6284) There's a general
bias to merging this, and calling the platform beta.

Some discussion about whether we should get a dedicate resource to
maintain servers and CI machines. seph is somewhat opposed, as running
servers incurs complex human costs over time.


## System Dependency Layer

The aarch stuff will need this. And the arch folks want it. And a
hypothetical ubuntu port would use it. Would also help LGTM.

The need to mix libraries from source, facebook, underlying operatring
systems is hard in the current system.

Stefano has been looking at this. He's thinking a lot about
simplifying it to be more cmake oriented. In that model, we'd pass
paths to cmake. (Via a bunch of `-D` options). We would, presumably,
ship a a set of configs for common OSes.

General theory is that we should:
1. Finish moving things to the source layer
2. Start moving to the more cmake oriented `-D` thing

## OpenSSL updated to 1.1.1d (https://github.com/osquery/osquery/pull/6302)

Some conversations about how we can't update the dependencies in
buck. So this PR updates cmake only. Discussion about whether or not
we need to keep buck -- https://github.com/osquery/osquery/issues/6305

Though the openssl API changed slightly, this PR uses `IFDEF` to
isolate those changes.

This PR also adds a libkafka fix. This needs verification from someone
who uses it.

## Third party libraries formulae simplified (https://github.com/osquery/osquery/pull/6303)

Alessandro says he originally make things really complex and all
encompossing. Stefano has been trying to simplify things.

Please help! Can people try building this branch?

## `--config_check` not exiting (https://github.com/osquery/osquery/issues/6290)

Stefano says he looked briefly. The `requestshutdown` call only works
cross thread, so places where we're calling it in the main thread
should be `shutdownnow`.

Discussion about whether `requestshutdown` can detect if we're in the
main thread, and then handle this. This approach is probably
reasonable, but we probably want to sleep on it.
