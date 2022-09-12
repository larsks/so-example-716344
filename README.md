This repository accompanies [this answer](https://unix.stackexchange.com/a/716379/4989) on unix.stackexchange.com.

To deploy the test environment using Podman:

```
podman run --replace --pull=always -d -p 127.0.0.1:3890:389 --name slapd \
  -v $PWD/config:/docker-entrypoint.d ghcr.io/larsks/docker-slapd-example:centos
```

Or Docker:

```
docker run --pull=always -d -p 127.0.0.1:3890:389 --name slapd \
  -v $PWD/config:/docker-entrypoint.d ghcr.io/larsks/docker-slapd-example:centos
```

This will give you a slapd server running on `localhost:3890`. You should be able to perform an `ldapsearch` like this:

```
ldapsearch -LLL -H ldap://localhost:3890 -x -D cn=admin,dc=yln,dc=info -w secret -b dc=yln,dc=info
```

Which will produce:

```
dn: dc=yln,dc=info
objectClass: organization
objectClass: dcObject
o: yln.info
dc: yln

dn: ou=users,dc=yln,dc=info
objectClass: organizationalUnit
ou: users

dn: cn=alice,ou=users,dc=yln,dc=info
objectClass: organizationalPerson
objectClass: simpleSecurityObject
cn: alice
sn: user
userPassword:: c2VjcmV0
```
