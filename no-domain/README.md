# logging in with no domain

Example of using the same user database for domain-less and domain
qualified users.

> [!WARNING]
> The dovecot.conf file provided here is insecure.

## config

Grab the minimal [dovecot.conf](./dovecot.conf) and userdb files.
and put them somewhere, perhaps `/tmp/dovecot`.

## run 

An example docker invocation, with configuration loaded from 
`/tmp/dovecot`, which also holds the `users.db` file.

```
docker run -p 1143:143 -p 1993:993 \
           -v /tmp/dovecot:/etc/dovecot \
              dovecot/dovecot:latest
```

## test

This does a lookup using `auth_username_format=%{user|username|lower}`.
Note that **no** default domain is used in this example.

```
$ nc 127.0.0.1 1143
* OK [CAPABILITY IMAP4rev1 LOGIN-REFERRALS ID ENABLE IDLE SASL-IR
  LITERAL+ AUTH=PLAIN] Dovecot ready.
a login tom test
a OK [CAPABILITY IMAP4rev1 SASL-IR LOGIN-REFERRALS ID ENABLE IDLE SORT
  ...trimmed...
  Logged in
```

This does a lookup using `auth_username_format=%{user|lower}`.

```
$ nc 127.0.0.1 1143
* OK [CAPABILITY IMAP4rev1 LOGIN-REFERRALS ID ENABLE IDLE SASL-IR
  LITERAL+ AUTH=PLAIN] Dovecot ready.
a login terry@another.com test
a OK [CAPABILITY IMAP4rev1 SASL-IR LOGIN-REFERRALS ID ENABLE IDLE SORT
  ...trimmed...
  Logged in
```

## log

See the [log](./log.txt).

