# go-ruby-ldap

**A pure-Go (CGO=0), MRI-faithful reimplementation of the Ruby
[`net-ldap`](https://github.com/ruby-ldap/ruby-net-ldap) gem's `Net::LDAP`
client surface** — `bind`, `search`, `Net::LDAP::Filter`, `Net::LDAP::Entry`,
`add` / `modify` / `delete` / `rename`, `get_operation_result` — layered over the
official pure-Go [`github.com/go-ldap/ldap/v3`](https://pkg.go.dev/github.com/go-ldap/ldap/v3)
transport.

It does **not** reimplement the LDAP protocol; it wraps the official client and
presents the `net-ldap` API, so a static, CGO=0 binary talks to a real directory
(OpenLDAP, Active Directory, 389 Directory Server, …).

## Install

```sh
go get github.com/go-ruby-ldap/ldap
```

## Quick start

```go
c, err := ldap.New(ldap.Config{
	Host:     "127.0.0.1",
	Port:     389,
	Base:     "dc=example,dc=com",
	Method:   "simple",
	Username: "cn=admin,dc=example,dc=com",
	Password: "secret",
})
if err != nil {
	log.Fatal(err)
}
defer c.Close()

if err := c.Bind(); err != nil {
	log.Fatalf("bind: %v (%+v)", err, c.OperationResult())
}

res, _ := c.Search(&ldap.SearchRequest{
	Scope:  ldap.ScopeSubtree,
	Filter: ldap.And(ldap.Eq("objectClass", "person"), ldap.Begins("cn", "al")),
})
for _, e := range res.Entries {
	fmt.Println(e.DN(), e.First("mail"))
}
```

## In rbgo

The same surface is available inside [rbgo](https://github.com/go-embedded-ruby/ruby)
as the `Net::LDAP` class:

```ruby
require "net/ldap"

ldap = Net::LDAP.new(host: "127.0.0.1", port: 389, base: "dc=example,dc=com",
                     auth: {method: :simple, username: "cn=admin,dc=example,dc=com", password: "secret"})
ldap.bind
ldap.search(filter: Net::LDAP::Filter.eq("cn", "alice")) { |entry| puts entry.dn }
```
