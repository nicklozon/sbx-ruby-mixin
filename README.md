# sbx-ruby-mixin

A Docker Sandboxes kit that installs Ruby into a sandbox. It is published on Docker Hub as [`lulzon/ruby`](https://hub.docker.com/r/lulzon/ruby).

## Usage

```sh
sbx run shell --kit lulzon/ruby --kit-arg ruby.version=4.0.9
```

## Arguments

| Argument       | Default | Description                                                               |
| -------------- | ------- | ------------------------------------------------------------------------- |
| `ruby.version` | `3.4.0` | Ruby release to install, in `X.Y.Z` form, as published by [jdx/ruby][rb]. |

[rb]: https://github.com/jdx/ruby

## What it does

- Downloads the prebuilt portable `ruby-<version>.<arch>_linux.tar.gz` tarball from [jdx/ruby][rb] and extracts it into `/usr/local`.
- Supports `x86_64` and `arm64`.
- Creates `/usr/local/bundle`, writable by any user, and sets `GEM_HOME` to it and adds `/usr/local/bundle/bin` to `PATH` (through `/etc/sandbox-persistent.sh`). The Ruby install itself stays root-owned, so `gem install` and `bundle install` work as the sandbox user without `sudo`.
- Allows outbound access to `rubygems.org:443` and `index.rubygems.org:443` at runtime so `gem` and `bundle` can install gems.

## Build tools are not included

This kit does not install a C compiler or any development libraries. Pure-Ruby gems install as-is, but a gem with a native extension fails to build until the tools it needs are installed manually, inside the sandbox.

## Requirements

- The release must exist in [jdx/ruby][rb], tagged `<version>`.
- The sandbox image needs `curl` and `tar`, and glibc 2.17 or newer.
- The workload must source `/etc/sandbox-persistent.sh` in its shells (Docker's `shell` workload and other `sbx@1` workloads do), or `GEM_HOME` and `PATH` won't be set.