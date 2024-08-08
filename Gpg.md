
# Gpg

## Verifying signatures

```
gpg --fetch-keys "https://www.mediawiki.org/keys/keys.txt"
```

if this wont work download separately and use:

```
gpg --import keys.txt
```

and finally 

```
 gpg --decrypt  mediawiki-1.38.2.tar.gz.sig 
```

Something like shoud be ok, (as don't have the full key hierarchy available) :

```
gpg: assuming signed data in 'mediawiki-1.38.2.tar.gz'
gpg: Signature made Do 30 Jun 2022 21:18:36 CEST
gpg:                using DSA key 1D98867E82982C8FE0ABC25F9B69B3109D3BB7B0
gpg: Good signature from "Sam Reed <reedy@wikimedia.org>" [unknown]
gpg: WARNING: This key is not certified with a trusted signature!
gpg:          There is no indication that the signature belongs to the owner.
Primary key fingerprint: 1D98 867E 8298 2C8F E0AB  C25F 9B69 B310 9D3B B7B0
