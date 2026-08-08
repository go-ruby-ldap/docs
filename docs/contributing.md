# Contributing

`go-ruby-ldap/ldap` is BSD-3-Clause and welcomes contributions.

## Building & testing

```sh
GOWORK=off go build ./...
GOWORK=off go vet ./...
gofmt -l .
GOWORK=off go test -race -cover ./...      # 100% coverage gate
cd live && go test ./...                    # live in-process LDAP server
```

- **Pure Go, CGO=0.** No cgo, no shelling out to external CLIs. A Go-module
  dependency is fine (the protocol is delegated to `github.com/go-ldap/ldap/v3`).
- **100% coverage**, including error branches, is enforced in CI across three
  host OSes and the six 64-bit targets under qemu.
- Public content is English; the licence is BSD-3-Clause, copyright "the
  go-ruby-ldap/ldap authors".
