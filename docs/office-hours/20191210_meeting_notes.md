# osquery office hours 2019-12-10

YouTube Link:

## Announcements and Highlights since the last meeting

There have been an ongoing stream of fixes to the package builds since
moving to cpack. Kudos to Teddy and Stefano for leading that.

## Any PRs people want to discuss

The intent of this section, is to allow contributors to bring up and
PRs they feel blocked on.

### NTFS Journal Events

https://github.com/osquery/osquery/pull/5371

Teddy thinks this looks okay from a code point of view. But he is not
familiar enough with the windows side.

Is there someone who can run some basic utilization testing for this?

## 4.1.2 

https://github.com/osquery/osquery/milestone/47

Various bugs, cpack updates

### Table Constraint Spec Issue

Fritz @ Kolide noticed that the [mdfind table stopped
working](https://github.com/osquery/osquery/issues/6099) This was due
to the change in how we pass constraints. The mdfind table was fixed
in https://github.com/osquery/osquery/pull/6103

We have some pending other fixes:
* https://github.com/osquery/osquery/pull/6104
* https://github.com/osquery/osquery/pull/6105

Tables need a primary key. Status quo, is that the primary key is the
union of `index` columns, or, if not present, to use the internally
generated `rowid`. The history around this is partly forgotten.

The intent of #6104 is to start cleaning this up. We think:

1. The underlying table implementions will only get column constraints
   that have `additional` or `index` set
2. `index` hints to sqlite that that column is indexed, and should be
   taken into account during join planning
3. `additional` is passed to the underlying implementation, but not be
   taken into query planning

And for primary keys:

| index columns | additional columns | resultant primary key |
| ------------- | ------------------ | --------------------- |
| Yes           | No                 | Union of index columns |
| Yes           | Yes                | Union of index and additional |
| No            | Yes                | Union of all columns  |
| No            | No                 | Internal sqlite rowid |

A good example of how this plays out, is the `routes` table. It has an
additional column which changes what's generated. But, it's not
indexed. However, if we use the internal `rowid` calling routes with
the different additional options, they'll overwrite eachother. Thus,
using all columns.

### Packaging fixes

Stefano and Teddy are leading this.

https://github.com/osquery/osquery/issues/6074


## Python Tests

https://github.com/osquery/osquery/pull/6068 merged. This doesn't have
any tests yet, but we'll probably see one soon.

##  Caching submodules; this should speed up the clone time quite a bit

The suggestion here is to add `.git/modules` to the azure cache. This
would cache the git objects, but not and of the c objects. We theorize
this will cut the configure step. No objections were raised.

## Windows MSI build in cpack

We're not sure if the MSI cpack side is all working. Need Nick's input

Probably we'll do some light cleanup on the existing make package
script in the interim.
* https://github.com/osquery/osquery/issues/6098
* https://github.com/osquery/osquery/issues/6109

## Watchdog blacklisting expensive queries

https://github.com/osquery/osquery/issues/6111

Right now, if the watchdog sees a schedule running amock, it will
blacklist the query for 24 hours. This functionality does not exist
for distributed queries.

Some conversation about what we want. There's general sentiment for
covering these in the blacklist. Some discussion about how to make
that work. Whether by query name or query sql.

## Disabling tables

seph mentions he was playing with hopw to disable tables to allow
extensions to create replacement tables. The existing
`--disable-tables` leaves them registered, which means an extension
can't overwrite them. There's some interest in allowing a full
`deregister`, which would allow an extension to then register them.

https://www.sqlite.org/c3ref/drop_modules.html recently shipped, which
might be an interesting tool

## Errors from queries

Zach started a conversation about how we return errors.

Right now, agent query status (both scheduled and distributed) return
results to the results endpoint, and the status to the log
endpoint. This can be hard to consume.

General brainstorming and discussion.

## c++ SDK Discussion

Right now, the c++ sdk very much lives inside the entire repo. This is
because extensions often use various helper functions around the code
base. Alessandro proposes making a build target to _not_ build
`osqueryd`. This would allow extensions to build inside the osquery
source tree, and not accidentally trigger a full build.

An alternative would be to make a minimal thrift based sdk. This
wouldn't have all the same helpers/utility functions, but would be
more easily supportable.

## Anything exciting happening?

Since we have some time, anyone want to share anything?

* Sharvil is thinking about a XXXX client
* Matt K. is planning on working on some bluetooth tables, windows first
* seph mentions some bugs in the underlying plist table.
* Alessando is doing neat ebf things [demo](https://asciinema.org/a/1sqjxFStdYo91mAPDdLFMSaa0)
