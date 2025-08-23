# migration to 2.4

Example of migrating a configuration to dovecot 2.4

> [!WARNING]
> This is untested in production.

## config

The original config is [here](dovecot.conf.orig).

## run 

```
docker run -p 1143:143 -p 1993:993 \
           -v ./:/etc/dovecot \
              dovecot/dovecot:latest
```

## test

Test with logins etc...

```
$ nc 127.0.0.1 1143
* OK [CAPABILITY IMAP4rev1 LOGIN-REFERRALS ID ENABLE IDLE SASL-IR
  LITERAL+ AUTH=PLAIN] Dovecot ready.
a login tom test
a OK [CAPABILITY IMAP4rev1 SASL-IR LOGIN-REFERRALS ID ENABLE IDLE SORT
  ...trimmed...
  Logged in
```

## log

See the [log](./log.txt).

