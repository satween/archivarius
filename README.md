### 1. Setup integration with yandex disk

Go [here](https://passport.yandex.ru/profile) -> "Passwords and authorization" -> Create an app password

### 2. Setup encryption
* Generate key, using command: 
`gpg --gen-key`

* Show key id using command:
`gpg --list-keys --keyid-format long`

*Example output:*
```
 pub   rsa3072/034542F74F3C8659 2021-12-11 [SC] [expires: 2023-12-11]
      B672DD58EB448D8BF9D12310034542F74F3C8659
 uid                 [ultimate] testmachine <testmachine@satween.net>
 sub   rsa3072/3A7EF27EAF0F26E4 2021-12-11 [E] [expires: 2023-12-11]
```

* Export key to file:
```
gpg --armor --export -a B672DD58EB448D8BF9D12310034542F74F3C8659  > public.key
gpg --armor --export-secret-keys -a B672DD58EB448D8BF9D12310034542F74F3C8659  > private.key
```


> If you do not need to restore backup frequently you can remove gpg key from currrent machine (don't forger to save exported files!) by command: `gpg --delete-secret-key 034542F74F3C8659`


> If `gpg` hangs on any command, try to restart agent:`gpgconf --kill gpg-agent`


### 3. Fill the configuration
```
DISK_USER=# Username for your yandex.dist account (example@yandex.ru)
DISK_PASSWORD=# Password, obtained on the first step of this instruction
GPG_KEY=# Id of the key, obtained on the second step of this instruction (034542F74F3C8659)
GPG_PASSWORD=# Optional password from secret gpg-key. Required only for restoring
DEST=# Destination of backup files on your yandex.disk
BACKUP_DIR=# Path of the main dir to backup (webdavs://${DISK_USER}:${DISK_PASSWORD}@webdav.yandex.ru/backup/testmachine)
INCLUDES_EXCLUDES="--exclude '/home/*/Downloads' \
    --exclude '/home/*/.cache' \
    --exclude '/home/*/Music' \
    --exclude '/home/*/pub' \
    --exclude '/home/*/tmp' \
    --exclude '/home/*/VirtualBox VMs' \
    --exclude /root/.cache
    --include /home \
    --include /root \
    --include /etc \
    --include /srv \
    --exclude '**'" # Optional additional path to backup or to exclude from backup
```
