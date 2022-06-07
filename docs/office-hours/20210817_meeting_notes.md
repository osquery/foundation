# osquery office hours 2021-08-17

YouTube Link: https://www.youtube.com/watch?v=jOtBrhnwvkY

## Announcements and Highlights since the last meeting

## Any Questions / Issues / PRs people want to discuss?

The intent of this section is to provide a clear time for community members to bring up _anything_.

Broad questions? Bugs? Deployment questions? Blocked PRs?

### Bulk import minutes from hackmd

seph usually writes in hackmd, and then imports to git. This is a massive backlog.

https://github.com/osquery/foundation/pull/79

### Puffycid has a bunch of PRs, we should review them

Some discussion about these

### Support pid_with_namespace in more tables

https://github.com/osquery/osquery/pull/7132

Stefano will try to review

## Conversation about dropping permission

A somewhat speculative conversation about what it would be like if osquery could drop permissions before reading untrusted data. No real conclusions here. But interesting conversation about how this intersects the pid_with_namespace

## State of 5.0

See discussion in https://github.com/osquery/osquery/pull/7263

Some testing around the macOS `.app` bundle found some problems with how we're code signging. We _had_ been signing the macho binary and then moving it into a `.app`. But, that breaks the signature verification.

There is a tradeoff here. We need to sign the `.app`, but people cannot extract a valid signed binary from it. So if we still want to distribute a signed, plain, macho binary, we need to do it seperately. (seph isn't sure we need to).

It is more correct to sign the `.app` as a bundle. Sharvil and seph are working through it.

Conclusion is that we think we should ship both. This allows the vendors who use a bare macho binary to keep shipping something with osquery signatures. (As we see some vendors ship modified osquery binaries, there is value here). But, we want to be cognizant of end user confusion. So while this might be a download on github, it will not be in a pkg, and will not be the easy path.

### Test macOS package via CI

https://github.com/osquery/osquery/pull/7263 and
https://github.com/osquery/osquery-packaging/pull/11

Would be super helpful to have test packages (off master perhaps), will instill some confidence if we have end-end package.

When we resolve paths, and merge PRs, we can run a build

### Other 5.0 blockers? (Sharvil)

Windows Certs?
Linux paths (wip i think)
Anything else I can help with?


## Structured logging of query/pack in results (zwass)


Currently osquery logs look like this:

```
{"snapshot":[{"foo":"bar"}],"action":"snapshot","name":"pack/test/test","hostIdentifier":"51329446-6053-4d0c-9628-4bd0158bae24","calendarTime":"Tue Aug 17 16:54:36 2021 UTC","unixTime":1629219276,"epoch":0,"counter":0,"numerics":false}
```

The `name` is generated from the query name, the configured `pack_delimiter`, and the name of the pack.

This can be difficult to parse programmatically with the possibly changing delimiter, ability for the user to include the delimiter in the query/pack name, etc.

Can we log this in a more structured form?

Such as (if we want to make a breaking change for 5.0)

`name` - Name of query
`pack` - Name of pack (if query is in pack)

Or if we want to make it non-breaking

`name` - Stays as-is
`query` - Name of query
`pack` - Name of pack (if query is in pack)

There is support for this. Preferably via the non-breaking route. There is a long tail of how people process logs in various third party systems. 

seph raises a related, https://github.com/osquery/osquery/issues/7037 But that is about status logs, and is a much deeper change. 

## CI for m1

What are our choices?

Does cross compilation work? We don't yet know if we'll have confidence in the binaries. 

Are there are services that will do this for us? MacStadium is known to be pretty generous.

## Misc

### LLVM and C++ introduction for new osquery contributors willing to contribute BPF code

https://www.trailofbits.com/post/all-your-tracing-are-belong-to-bpf