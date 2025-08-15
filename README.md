# dovecot 2.4 config

Minimal docker setup for testing dovecot 2.4 configurations.

> [!WARNING]
> The dovecot.conf file provided here is insecure.

Contents

* [config](#config)
* [run](#run)
* [test](#test)
* [examples](#examples)
* [notes](#notes)

## config

Grab the minimal [dovecot.conf](./dovecot.conf) and [users.db](./users.db)
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

Example login with no domain, using `auth_default_domain`

```
$ nc 127.0.0.1 1143
* OK [CAPABILITY IMAP4rev1 LOGIN-REFERRALS ID ENABLE IDLE SASL-IR
  LITERAL+ AUTH=PLAIN] Dovecot ready.
a login tom test
a OK [CAPABILITY IMAP4rev1 SASL-IR LOGIN-REFERRALS ID ENABLE IDLE SORT
  ...trimmed...
  Logged in
```

Example login with specified domain:

```
$ nc 127.0.0.1 1143
* OK [CAPABILITY IMAP4rev1 LOGIN-REFERRALS ID ENABLE IDLE SASL-IR
  LITERAL+ AUTH=PLAIN] Dovecot ready.
a login terry@another.com test
a OK [CAPABILITY IMAP4rev1 SASL-IR LOGIN-REFERRALS ID ENABLE IDLE SORT
  ...trimmed...
  Logged in
```

The dovecot config provided here logs to `/dev/stderr`, and produces
output along the lines below. Note the expansion to `tom@example.com`.

```
Aug 15 17:05:53 master: Info: Dovecot v2.4.1 (7d8c0e5759) starting up for imap
Aug 15 17:05:57 auth: Debug: Loading modules from directory: /dovecot/lib/dovecot/modules/auth
...
Aug 15 17:05:57 auth: Debug: Wrote new auth token secret to /run/dovecot/auth-token-secret.dat
Aug 15 17:05:57 auth: Debug: passwd-file /etc/dovecot/users.db:Read 3 users in 0 secs
...
Aug 15 17:06:02 auth: Debug: conn unix:login (pid=12,uid=1000) [1]: client in: AUTH	1	PLAIN	protocol=imap	final-resp-ok	session=Cd5NZ2o8ZpysEQAB	lip=172.17.0.2	rip=172.17.0.1	lport=143	rport=40038	resp=<hidden>
Aug 15 17:06:02 auth(tom@example.com,172.17.0.1,sasl:plain)<Cd5NZ2o8ZpysEQAB>: Debug: passwd-file: Performing passdb lookup
Aug 15 17:06:02 auth(tom@example.com,172.17.0.1,sasl:plain)<Cd5NZ2o8ZpysEQAB>: Debug: passwd-file: lookup: user=tom@example.com file=/etc/dovecot/users.db
Aug 15 17:06:02 auth(tom@example.com,172.17.0.1,sasl:plain)<Cd5NZ2o8ZpysEQAB>: Debug: passwd-file: Finished passdb lookup
Aug 15 17:06:02 auth(tom@example.com,172.17.0.1,sasl:plain)<Cd5NZ2o8ZpysEQAB>: Debug: Auth request finished
Aug 15 17:06:02 auth: Debug: conn unix:login (pid=12,uid=1000) [1]: client passdb out: OK	1	user=tom@example.com	:=	original_user=tom
Aug 15 17:06:02 auth: Debug: conn unix:/run/dovecot/auth-master (pid=15,uid=1000): Server accepted connection (fd=20)
Aug 15 17:06:02 auth: Debug: master in: REQUEST	3342336001	12	1	c32446853f20868faf73f76a286beab0	session_pid=15	request_auth_token
Aug 15 17:06:02 auth(tom@example.com,172.17.0.1,sasl:plain)<Cd5NZ2o8ZpysEQAB>: Debug: passwd-file: Performing userdb lookup
Aug 15 17:06:02 auth(tom@example.com,172.17.0.1,sasl:plain)<Cd5NZ2o8ZpysEQAB>: Debug: passwd-file: lookup: user=tom@example.com file=/etc/dovecot/users.db
Aug 15 17:06:02 auth(tom@example.com,172.17.0.1,sasl:plain)<Cd5NZ2o8ZpysEQAB>: Debug: passwd-file: Finished userdb lookup
Aug 15 17:06:02 auth: Debug: master userdb out: USER	3342336001	tom@example.com	uid=1000	gid=1000	home=/srv/mail/tom	auth_mech=PLAIN	auth_token=1da9be7cba83819ae2d93a33e448d52ddeec144f	auth_user=tom
Aug 15 17:06:02 imap-login: Info: Logged in: user=<tom@example.com>, method=PLAIN, rip=172.17.0.1, lip=172.17.0.2, mpid=15, session=<Cd5NZ2o8ZpysEQAB>
```

## examples

Other examples:

* [two-userdbs](./two-userdbs/)  
  use of two different userdbs, each both for the user and pass
  databases.
* [no-domain](./no-domain/)  
  use of the same userdb, both for domain-less and "with" domain logins.

## notes

The main configuration reference for Dovecot CE 2.4.x is
[here](https://doc.dovecot.org/2.4.1/core/settings/variables.html).

Have a look at the [Dovecot testing
guide](https://doc.dovecot.org/main/core/admin/testing.html).

The "Upgrading Dovecot CE from 2.3 to 2.4" docs are
[here](https://doc.dovecot.org/main/installation/upgrade/2.3-to-2.4.html)
and include a link to an example config for 2.4 at
https://github.com/dovecot/tools/blob/main/dovecot-2.4.0-example-config.tar.gz

The configuration included with the docker image can be read by using
`docker container export ...` which writes to a tar file.

## licence

MIT
