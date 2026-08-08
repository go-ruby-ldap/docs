# Why pure Go

`go-ruby-ldap` is **CGO=0**: it links no C libraries and shells out to no
external `ldapsearch` binary. LDAP protocol work is delegated to the official
pure-Go client [`github.com/go-ldap/ldap/v3`](https://pkg.go.dev/github.com/go-ldap/ldap/v3);
this module adds the `net-ldap` ergonomics and result/error model on top.

- **Cross-compiles anywhere.** A static binary by default; six 64-bit Go targets
  (amd64, arm64, riscv64, loong64, ppc64le, big-endian s390x) plus `js/wasm` and
  `wasip1/wasm` build lanes.
- **Reference-faithful.** The public `Net::LDAP` API — `bind`, `search`, the
  `Filter` builder (RFC 4515), the case-insensitive `Entry`, `get_operation_result`
  — reproduced.
- **Standalone & reusable.** No Ruby runtime dependency; the dependency runs the
  other way — `rbgo` binds *this* module.

## Transport is a host seam

A `Client` drives an injected transport whose method set is satisfied directly by
`*ldap.Conn` (no adapter). Every method's request-building and response-mapping
logic is therefore testable against a **deterministic in-memory transport** — no
external directory, no cgo — so the suite holds **100% coverage on every arch
under qemu**. A separate live suite drives an in-process, pure-Go LDAP server for
real round-trip validation on native lanes.
