# Golang Resources

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

    