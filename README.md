# Encrypting Home Directory in Fedora 40
The following steps, taken from [Tao of Mac](https://taoofmac.com/space/notes/2023/08/28/1900), could be used as of Fedora 38 to encrypt a user's home folder, after Fedora changed from `authconfig` to `authselect`. However, as of Fedora 40 it is not working anymore, due to changes in the naming of the default profiles. The following should work as of Fedora 40.

These commands need to be run from a root shell.

```
# install necessary software
dnf install ecryptfs-utils
```

Enabling the ecryptfs module is necessary, either by rebooting or by executing
```
modprobe ecryptfs
```

To enable the home folder encryption of user `username`, run the following. Replace `username` by the desired user. Ensure that the user for which the changes are being made is not logged in.

```
authselect select local with-ecryptfs with-pamaccess
usermod -aG ecryptfs username

ecryptfs-migrate-home -u username

su - username
ecryptfs-unwrap-passphrase ~/.ecryptfs/wrapped-passphrase
ecryptfs-insert-wrapped-passphrase-into-keyring ~/.ecryptfs/wrapped-passphrase
```

