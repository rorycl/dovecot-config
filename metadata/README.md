# imap metadata

Example of using the `imap_metadata` flag for imap.

See https://doc.dovecot.org/2.4.1/core/config/imap.html#metadata.

> [!NOTE]
> The imap_metadata flag appears not to be working.

An example docker invocation, with configuration loaded from `this
directory`, which also holds the `users.db` file.

## test

Run dovecot:

```
docker run -p 1143:143 -p 1993:993 -v ./:/etc/dovecot dovecot/dovecot:latest
Aug 23 12:34:16 master: Info: Dovecot v2.4.1 (7d8c0e5759) starting up for imap
```

Check config by running `doveconf` in the container at `/dovecot/bin`:

```
docker exec \
       $(docker ps --filter="ancestor=dovecot/dovecot:latest" --format "{{.Names}}") \
       /dovecot/bin/doveconf -a | \
       grep -C 3 imap_metadata
```

Output:

```
imap_id_send = 
imap_literal_minus = no
imap_logout_format = in=%{input} out=%{output} deleted=%{deleted} expunged=%{expunged} trashed=%{trashed} hdr_count=%{fetch_hdr_count} hdr_bytes=%{fetch_hdr_bytes} body_count=%{fetch_body_count} body_bytes=%{fetch_body_bytes}
imap_metadata = no
imap_urlauth_host = 
imap_urlauth_logout_format = in=%{input} out=%{output}
imap_urlauth_port = 143
--
welcome_wait = no
protocol imap {
  mail_plugins {
    imap_metadata = yes
  }
}
namespace inbox {
```

The `imap_metadata` flag is confirmed as `yes` for this example.

The following error is recorded in the logs:

```
Aug 23 13:29:36 imap(tom): Error: Plugin 'imap_metadata' not found from directory /dovecot/lib/dovecot/modules
```

## log

See the [log](./log.txt).

