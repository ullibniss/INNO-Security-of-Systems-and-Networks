# Lab 1. Classical Crypto


## Done by Fedorov Alexey (<a href="https://t.me/ullibniss"><img src="https://upload.wikimedia.org/wikipedia/commons/8/83/Telegram_2019_Logo.svg" width="40"></img></a><a href="https://github.com/ullibniss/"><img src="https://user-images.githubusercontent.com/25181517/192108374-8da61ba1-99ec-41d7-80b8-fb2f7c0a4948.png" alt="drawing" width="40"></img></a>)

---

## Task 1. Firmware Databases.
```
1. Extract the Microsoft certificate that belongs to the key referred to in Step 1 from the UEFI firmware, and show its text representation.
Hint: efitools, openssl x509
```

```
2. Is this certificate the root certificate in the chain of trust? What is the role of the Platform Key (PK) and other variables?
```

## Task 2. SHIM
```
3. Verify that the system indeed boots the shim boot loader in the first stage. What is the full path name of this bootloader?
Hint: efibootmgr
```

```
4. Verify that the shim bootloader is indeed signed with the “Microsoft Corporation UEFI CA” key.
Hint: sbsigntool, PEM format, pesign, sbverify
```

```
5. What is the exact name of the part of the binary where the actual signature is stored?
```

```
6. In what standard cryptographic format is the signature data stored?
```

```
7. Extract the signature data from the shim binary using dd. Add 8 bytes to the location
as given in the data directory to skip over the Microsoft WIN CERTIFICATE
structure header (see page 14 of the specification if you are interested).
Hint: pyew.py, -/+8.
```

```
8. Show the subject and issuer of all X.509 certificates stored in the signature data.
Draw a diagram relating these certificates to the “Microsoft Corporation UEFI CA”
certificate.
Hint: binwalk
```

## Task 3. GRUB (BONUS)

```
9. Using your new knowledge about Authenticode binaries, extract the signing
certificates from the GRUB boot loader, and show the subject and issuer.
```

```
10. Why is storing the certificate X in the shim binary secure?
```

```
11. What do you think is the subject CommonName (CN) of this X certificate?
```

```
12. Obtain the X certificate used by the shim to verify the GRUB binary. There are two ways to obtain it: from the source code or from the binary directly.
```

```
13. Verify that this X certificate’s corresponding private key was indeed used to sign the GRUB binary.
```

## Task 4. The Kernel

```
14. Verify the kernel you booted against the X certificate.
```

```
15. Where does GRUB get its trusted certificate from? Hint: It is not stored in the binary, and it is not stored on the file system.
```

```
16. Draw a diagram that shows the chain of trust from the UEFI PK key to the signed kernel. Show all certificates, binaries, and signing relations involved.
```
