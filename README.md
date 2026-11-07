# sbx-ruby-mixin

A Docker Sandboxes kit that installs Ruby into a sandbox. It is published on Docker Hub as [`lulzon/ruby`](https://hub.docker.com/r/lulzon/ruby).

## Usage

```sh
sbx run shell --kit lulzon/ruby --kit-arg ruby.version=4.0.9
```

## Arguments

| Argument       | Default | Description                                                                          |
| -------------- | ------- | ------------------------------------------------------------------------------------ |
| `ruby.version` | `3.4.0` | Ruby release to install, in `X.Y.Z` form, as published by [ruby/ruby-builder][rb]. |

[rb]: https://github.com/ruby/ruby-builder

## What it does

- Downloads the prebuilt `ruby-<version>` tarball from [ruby/ruby-builder][rb] and extracts it into `/usr/local`.
- Supports `x86_64` and `arm64`.
- Allows outbound access to `rubygems.org:443` and `index.rubygems.org:443` at runtime so `gem` and `bundle` can install gems.

## Requirements

- The release must exist in [ruby/ruby-builder][rb] as `ruby-<version>`.
- The sandbox image needs `curl` and `tar`, and glibc 2.35 or newer. The binaries are built on Ubuntu 22.04.

## Files

- `ruby.yaml`: the kit definition.
