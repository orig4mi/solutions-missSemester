[Home](README.md)

# Lecture 9: Security and Cryptography


1. > **Entropy.**
    1. > Suppose a password is chosen as a concatenation of four lower-case
       > dictionary words, where each word is selected uniformly at random from a
       > dictionary of size 100,000. An example of such a password is
       > `correcthorsebatterystaple`. How many bits of entropy does this have?
       >
       Total of possible passwords: (10\*\*5)\*\*5 = 10\*\*25.
       Entropy: log\_2(10\*\*25) = 83.05 bits
 
    1. > Consider an alternative scheme where a password is chosen as a sequence
       > of 8 random alphanumeric characters (including both lower-case and
       > upper-case letters). An example is `rg8Ql34g`. How many bits of entropy
       > does this have?
       >
       Total of possible passwords: (26 + 26 + 10)\*\*8 = 62\*\*8.
       Entropy: log\_2(62\*\*8) = 47.63
   
    1. > Which is the stronger password?
       >
       The first one.

    1. > Suppose an attacker can try guessing 10,000 passwords per second. On
       > average, how long will it take to break each of the passwords?
       >
       Case 1: 31.7 trillion years (31.7\*10\*\*12).
       Case 2: 692 years.
 
1. > **Cryptographic hash functions.** Download a Debian image from a
   > [mirror](https://www.debian.org/CD/http-ftp/) (e.g. [from this Argentinean
   > mirror](http://debian.xfree.com.ar/debian-cd/current/amd64/iso-cd/).
   > Cross-check the hash (e.g. using the `sha256sum` command) with the hash
   > retrieved from the official Debian site (e.g. [this
   > file](https://cdimage.debian.org/debian-cd/current/amd64/iso-cd/SHA256SUMS)
   > hosted at `debian.org`, if you've downloaded the linked file from the
   > Argentinean mirror).
   >
```bash
~$ sha256sum debian-10.4.0-amd64-netinst.iso
ab3763d553330e90869487a6843c88f1d4aa199333ff16b653e60e59ac1fc60b  debian-10.4.0-amd64-netinst.iso
```
1. > **Symmetric cryptography.** Encrypt a file with AES encryption, using
   > [OpenSSL](https://www.openssl.org/): `openssl aes-256-cbc -salt -in {input
   > filename} -out {output filename}`. Look at the contents using `cat` or
   > `hexdump`. Decrypt it with `openssl aes-256-cbc -d -in {input filename} -out
   > {output filename}` and confirm that the contents match the original using
   > `cmp`.
   >
```bash
~$ openssl aes-256-cbc -salt -in test -out test.encrypted
~$ hexdump -C test.encrypted
~$ openssl aes-256-cbc -salt -in test.encrypted -out test.decrypted
~$ cmp test test.decrypted
~$ echo $?
```
1. > **Asymmetric cryptography.**
    1. > Set up [SSH
       > keys](https://www.digitalocean.com/community/tutorials/how-to-set-up-ssh-keys--2)
       > on a computer you have access to (not Athena, because Kerberos interacts
       > weirdly with SSH keys). Rather than using RSA keys as in the linked
       > tutorial, use more secure [ED25519
       > keys](https://wiki.archlinux.org/index.php/SSH_keys#Ed25519). Make sure
       > your private key is encrypted with a passphrase, so it is protected at
       > rest.
>
Already done in [exercises for Lecture 5](exercises_5.md), section Remote Machines
    1. > [Set up GPG](https://www.digitalocean.com/community/tutorials/how-to-use-gpg-to-encrypt-and-sign-messages)
>
```bash
~$ gpg --full-generate-key
```
    1. > Send Anish an encrypted email ([public key](https://keybase.io/anish)).
>
Skipped

    1. > Sign a Git commit with `git commit -S` or create a signed Git tag with
       > `git tag -s`. Verify the signature on the commit with `git show
       > --show-signature` or on the tag with `git tag -v`.
>
Get key:
```bash
~$ gpg -K --keyid-format LONG
```
Tell git:
```bash
git config --global user.signingkey 8C3A3C5D197CCD22  
```
(note: the user name and email in git has to match those in gpg).

        Set the terminal (otherwise you will not see gpg ask for passphrase):
        ```bash
        ~$ export GPG_TTY=$(tty)     
        ```
        Now signed commits may be done.

        I added to .bashrc:
        ```bash
        GPG_TTY=$(tty)
        export GPG_TTY     
        ```
