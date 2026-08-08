# Reference

## Connection

| net-ldap gem                              | go-ruby-ldap/ldap                                    |
| ----------------------------------------- | ---------------------------------------------------- |
| `Net::LDAP.new(host:, port:, base:, ...)` | `ldap.New(ldap.Config{...})`                         |
| `ldap.bind`                               | `c.Bind()`                                           |
| `ldap.search(filter:, base:) { \|e\| }`   | `c.Search(&ldap.SearchRequest{...})` / `SearchEach`  |
| `ldap.add(dn:, attributes:)`              | `c.Add(dn, attrs)`                                   |
| `ldap.modify(dn:, operations:)`           | `c.Modify(dn, ops)`                                  |
| `ldap.delete(dn:)`                        | `c.Delete(dn)`                                       |
| `ldap.rename(olddn:, newrdn:)`            | `c.Rename(olddn, newrdn, true, "")`                  |
| `ldap.get_operation_result`               | `c.OperationResult()`                                |

## Search scopes

- `ldap.ScopeBase` — the base object only (`Net::LDAP::SearchScope_BaseObject`)
- `ldap.ScopeSingleLevel` — the base's immediate children
- `ldap.ScopeSubtree` — the base and its whole subtree (the net-ldap default)

## Filters (`Net::LDAP::Filter`)

| net-ldap                                  | go-ruby-ldap                        |
| ----------------------------------------- | ----------------------------------- |
| `Filter.eq(attr, v)`                      | `ldap.Eq(attr, v)`                  |
| `Filter.present(attr)` / `.pres`          | `ldap.Present(attr)`                |
| `Filter.ge(attr, v)` / `.le`              | `ldap.Ge` / `ldap.Le`               |
| `Filter.begins` / `ends` / `contains`     | `ldap.Begins` / `Ends` / `Contains` |
| `f & g` / `f \| g` / `~f`                  | `ldap.And` / `ldap.Or` / `ldap.Not` |
| `Filter.construct(str)` / `from_rfc2254`  | `ldap.Construct(str)`               |

Assertion values are escaped per RFC 4515.

## Entry (`Net::LDAP::Entry`)

Attribute access is case-insensitive: `Entry.DN()`, `Entry.Get(attr) []string`,
`Entry.First(attr) string`, `Entry.AttributeNames()`.

## Modify operations

`ldap.ModAdd`, `ldap.ModReplace`, `ldap.ModDelete` — mirroring net-ldap's
`:add` / `:replace` / `:delete` operation symbols.

## Errors

`ldap.Error` carries the LDAP result code and a net-ldap-style `Name`
(`NoSuchObject`, `InvalidCredentials`, `EntryAlreadyExists`, …), plus synthetic
`Network` and `FilterSyntax` codes. Match with `errors.Is(err, ldap.ErrNoSuchObject)`.
In rbgo these map to the `Net::LDAP::Error` exception tree.
