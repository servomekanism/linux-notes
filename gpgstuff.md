[//]: # (tags: gpg encryption generate keys key management import export secret private public keyring)

# GPG Key Management Cheat Sheet #
Quick'n easy gpg cheatsheet

### to create a key:
`gpg --gen-key`

### to export a public key into file public.key
`gpg --export -a "someone@gmail.com" > public.key`

### to export a private key
`gpg --export-secret-key -a "someone@gmail.com" > private.key`

### to import a public key
`gpg --import public.key`

### to import a private key (obsolete)
`gpg --allow-secret-key-import --import private.key`

### to delete a public key (from your public keyring)
<<<<<<< HEAD
`gpg --delete-key "someone@gmail.com"` (Note: if there is a priv key on your priv keyring associated with this
public key you get an error. You must delete your private key for this key pair from your priv keyring first.
=======
`gpg --delete-key "someone@gmail.com"` (Note: if there is a priv key on your priv keyring associated with this public key you get an error. You must delete your private key for this key pair from your priv keyring first.)
>>>>>>> cd7c1ae25deb6389f6d87cea77869e8a63e63f47

### to delete a private key (from your priv keyring)
`gpg --delete-secret-key "someone@gmail.com"`

### to list public keys
`gpg --list-keys`

### to list secret (private) keys
`gpg --list-secret-keys`

### to generate a short list of numbers to verify each public key
`gpg --fingerprint > fingerprint`

### to encrypt data
`gpg -e -u "someone@gmail.com" -r "someoneelse@gmail.com" somefile`

### to decrypt data
`gpg -d mydata.tar.gpg`

