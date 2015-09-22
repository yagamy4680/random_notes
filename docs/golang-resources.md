# Golang Resources

## [go-coreutils](https://github.com/EricLagerg/go-coreutils)

A cross-platform port of GNU's coreutils to Go

16/100

| Utility | Completeness   | Cross Platform      | Need Refactor|
|:--------|:---------------|:--------------------|:-------------|
| cat     | 100%           | Yes (Unix/Windows)  | No           |
| chown   | 10% (* see note #1) | No             | Yes (-R)     |
| env     | 100%           | Yes (Unix/Windows)  | No           |
| false   | 100%           | Yes (Unix/Windows)  | No           |
| logname | 100%           | No                  | No           |
| pwd     | 100%           | Yes (Unknown)       | No           |
| sync    | 100%           | Yes (Unix/Windows)  | No           |
| true    | 100%           | Yes (Unix/Windows)  | No           |
| tsort   | 100%           | Yes (Unix/Windows)  | No           |
| tty     | 100%           | Yes (Unix/Windows)  | No           |
| uname   | 100%           | No                  | No           |
| uptime  | 90%            | Yes (Unix/Window, no FreeBSD)   | No           |
| wc      | 100%           | Yes (Unix/Windows)  | No           |
| whoami  | 100%           | Yes (Unix/Windows   | No           |
| xxd     | 100%           | Yes (Unix/Windows)  | No           |
| yes     | 100%           | Yes (Unix/Windows)  | No           |


## [godebug](https://github.com/mailgun/godebug)

A cross-platform debugger for Go.


## [gox](https://github.com/mitchellh/gox)

A dead simple, no frills Go cross compile tool

Gox is a simple, no-frills tool for Go cross compilation that behaves a lot like standard go build. Gox will parallelize builds for multiple platforms. Gox will also build the cross-compilation toolchain for you.

```text
$ gox
Number of parallel builds: 4

-->      darwin/386: github.com/mitchellh/gox
-->    darwin/amd64: github.com/mitchellh/gox
-->       linux/386: github.com/mitchellh/gox
-->     linux/amd64: github.com/mitchellh/gox
-->       linux/arm: github.com/mitchellh/gox
-->     freebsd/386: github.com/mitchellh/gox
-->   freebsd/amd64: github.com/mitchellh/gox
-->     openbsd/386: github.com/mitchellh/gox
-->   openbsd/amd64: github.com/mitchellh/gox
-->     windows/386: github.com/mitchellh/gox
-->   windows/amd64: github.com/mitchellh/gox
-->     freebsd/arm: github.com/mitchellh/gox
-->      netbsd/386: github.com/mitchellh/gox
-->    netbsd/amd64: github.com/mitchellh/gox
-->      netbsd/arm: github.com/mitchellh/gox
-->       plan9/386: github.com/mitchellh/gox
```

## [gb](http://getgb.io/)

A project based build tool for the Go programming language

- Reliable, reproducible, builds without import rewriting
- No enviroment variables to set
- Multiple working copies without changing environment variables
- Backwards compatible with `$GOPATH`


## [minio](https://minio.io/)

![](https://minio.io/img/logo.svg)

Apache License v2 - Amazon S3 Compatible - Written in Go

## Articles

[Scope and Shadowing in Go](http://blog.charmes.net/2015/06/scope-and-shadowing-in-go.html)

