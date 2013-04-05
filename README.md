opscode-bifrost cookbook
======================

This cookbook installs and configures [oc_bifrost](https://github.com/opscode/oc_bifrost), the authorization API for Opscode services.

Requirements
============

* [erlang_binary][] - Opscode's internal cookbook for installing an Erlang
  VM, as well as [rebar][], the Erlang build tool.
* [opscode-postgresql][] - Opscode's wrapper around the community
  [postgresql][] cookbook.  Defines common configuration and
  filesystem layout options for our infrastructure.
* [perl][] - for installing / running [pg_prove][]
* [git][] - for retrieving `oc_bifrost` and [pgTAP][] source code
* [python][] - for building [pgxnclient][], to install [pgTAP][]

Usage
=====

Attributes
==========

* `node['oc_bifrost']['user']` - The owner of the `oc_bifrost` server process
* `node['oc_bifrost']['group']` - The group of the `oc_bifrost` server process
* `node['oc_bifrost']['revision']` - The Git branch / tag / SHA1 of
  the source code to fetch.
* `node['oc_bifrost']['database']['name']` - The name of the
  database.  Defaults to `bifrost`.
* `node['oc_bifrost']['database']['users']['owner']['name']` - The
  PostgreSQL user that owns the database.
* `node['oc_bifrost']['database']['users']['owner']['password']` -
  The password for the database owner.
* `node['oc_bifrost']['database']['users']['read_only']['name']` -
  A PostgreSQL database user with read-only permissions on the
  database.  Good for use by service engineers and on-call engineers
  that need to safely explore and debug the database.
* `node['oc_bifrost']['database']['users']['read_only']['password']`
  - The password for the read-only database user.

Recipes
=======

* `common_directories` - Creates a common directory for source
  checkouts.  Could be extracted to a common platform-wide cookbook.
* `database` - Installs and configures a PostgreSQL server.  Creates
  database user accounts, the `bifrost` database, and migrates the
  database schema.
* `database_test` - In addition to everything that `database` does,
  installs [pgTAP][] and [pg_prove][] and prepares the database for
  running tests.
* `default` - Installs Erlang and rebar, fetches the `oc_bifrost` code,
  and installs the database.
* `development` - Development mode.  Use this recipe when hacking on
  `oc_bifrost` locally via Vagrant.  Takes source code and schema from
  the `/vagrant` directory instead of fetching from Github.
* `fetch_code` - Grab the `oc_bifrost` code from Github __if not in
  development mode__.
* `pg_prove` - Install [pg_prove][].  Will eventually be added to the
  [postgresql][] cookbook.
* `pgtap` - Install [pgTAP][].  Will eventually be added to the
  [postgresql][] cookbook.

# Author

- Author:: Christopher Maier (<cm@opscode.com>)


[erlang_binary]:https://github.com/opscode/opscode-platform-cookbooks/tree/rs-prod/cookbooks/erlang_binary
[rebar]:https://github.com/basho/rebar
[opscode-postgresql]:https://github.com/opscode/opscode-platform-cookbooks/tree/rs-prod/cookbooks/opscode-postgresql
[postgresql]:https://github.com/opscode-cookbooks/postgresql
[perl]:https://github.com/opscode-cookbooks/perl
[pg_prove]:http://search.cpan.org/~dwheeler/TAP-Parser-SourceHandler-pgTAP-3.29/bin/pg_prove
[git]:https://github.com/opscode-cookbooks/git
[python]:https://github.com/opscode-cookbooks/python
[pgxnclient]:http://pgxnclient.projects.pgfoundry.org
[pgTAP]:http://pgtap.org
