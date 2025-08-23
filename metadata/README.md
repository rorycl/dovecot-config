# imap metadata

Example of using the `imap_metadata` flag for imap.

See https://doc.dovecot.org/2.4.1/core/config/imap.html#metadata.
# logging in with no domain

An example docker invocation, with configuration loaded from 
`this directory`, which also holds the `users.db` file.

## test

`docker run -p 1143:143 -p 1993:993 -v ./:/etc/dovecot dovecot/dovecot:latest`
Aug 23 12:34:16 master: Info: Dovecot v2.4.1 (7d8c0e5759) starting up for imap

## log

(no errors)

See the [log](./log.txt).

