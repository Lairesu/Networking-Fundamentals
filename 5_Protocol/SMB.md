# Server Message Block (SMB)

Similarly to FTP(file transfer protocol), Its a protocol for sharing files, sharing printers, sharing resources between computer on network.
Its mainly Windows Protocol. windows uses it everywhere
linux can use it via Samba

SMB is Similar but:

- windows focused
- shares folders not just files
- called "shares"
- can also be anonymous
- or with credentials

## Ports

**445/tcp** = SMB (modern)
**139/tcp** = SMB (older, NetBIOS)

## Tools for SMB

- smbclient = like FTP client for SMB
- submap = map SMB shares
- enum4linux = enumerate SMB info
- crackmapexec = Swiss army knife for SMB

>Note:
>
> can download file using ``get <filename>``