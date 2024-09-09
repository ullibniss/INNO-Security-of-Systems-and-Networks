# Lab-2: UEFI-Secure-Boot

## Done by Fedorov Alexey

---

## Prerequisits

![video-30](https://github.com/user-attachments/assets/9ca939c9-2341-4f3f-858f-d9bbfa42e095)


## Task 1. Firmware Databases

### 1.1.  Extract the Microsoft certificate that belongs to the key referred to in Step 1 from the UEFI firmware, and show its text representation. Hint: efitools, openssl x509

To do this, I need to install efitools. 

![image](https://github.com/user-attachments/assets/6c1966a5-65fc-4829-8221-a7401c901d25)


Then I use efi-readvar to get the certificate: 

`efi-readvar -v KEK -o old_kek.esl`

![image](https://github.com/user-attachments/assets/ff25ff17-5c06-4bf2-a0f1-4c5a2701c141)


efi-readvar - tool for showing secure variables.

The efitools package also contains the `sig-list-to-certs` tool that allows to extract DER-encoded X.509 certificates from the .esl files: 

`sig-list-to-certs old_kek.esl oldkek`

![image](https://github.com/user-attachments/assets/e916b77c-cdfb-4d20-bf83-e01a9cfb0f01)

Now I can see the certificate with the command:

`openssl x509 -in oldkek-0.der -inform der -noout -text` (Figures 1 and 2).

![image](https://github.com/user-attachments/assets/2da562ce-cb15-4056-94d2-609dab899b8a)

![image](https://github.com/user-attachments/assets/91f18c08-c357-4d4d-add5-30aacc9b8ee1)


### 1.2.  Is this certificate the root certificate in the chain of trust? What is the role of the Platform Key (PK) and other variables?

This certificate isn’t the root certificate.

Hardware manufacturers put what’s known as a Platform Key (PK) into the firmware, representing the Root of Trust. The trust relationship with operating system vendors and others is documented by signing their keys with the Platform Key.

The first X509 certificate in the chain is authenticated against values burnt into the hardware and each subsequent certificate is authenticated against the prior certificate. This chain of X509 certificates finally authenticates a public key that is used to verify the digital signature, which proves the correctness of the hash table and image metadata.

- **DB** (signature database) - contains the trusted keys used for authenticating any applications or drivers executed in the UEFI environment.  
- **DBX** (signature database blacklist) - contains a set of explicitly untrusted keys and binary hashes. Any application or driver signed by these keys or matching these hashes will be blocked from execution.  
- **KEK** (key exchange keys database) - contains the set of keys trusted for updating DB and DBX.  
- **PK** (platform key) - Only updates signed with PK can update the KEK database.


## Task 2. SHIM


### 2.3.  Verify that the system indeed boots the shim boot loader in the first stage. What is the full path name of this boot loader?

For this purpose, I used the efibootmgr - tool for manipulating the UEFI Boot Manager (Figure 3)

![image](https://github.com/user-attachments/assets/893c56fe-d481-4391-82fe-c37a3967d737)

Boot order started with 0000 - ubuntu, which has a bootloader shim, that is located at `/boot/efi/EFI/ubuntu/shimx64.efi`.


### 2.4.  Verify that the shim bootloader is indeed signed with the “Microsoft Corporation UEFI CA” key. Hint: sbsigntool, PEM format

I can see a list of all signatures using the command `sbverify --list` as shown in Figure 4.

![image](https://github.com/user-attachments/assets/a97eaffc-3d3c-4912-bde7-3b1e210f5494)


To verify the shim bootloader I need a PEM certificate.

So first I extract the keys from the signature database by using `mokutil --export --db`. As a result, I received six files in der format (Figure 5).

![image](https://github.com/user-attachments/assets/f4f43e89-ce71-4600-9b9a-1b42d8af24cf)

Then I converted them to pem format as shown below.

![image](https://github.com/user-attachments/assets/cacc0557-7a46-4f3c-b1af-478bd34b9fe0)

Now let's print the common names from the certificates. Figure 6 shows the result of executing commands.

![image](https://github.com/user-attachments/assets/e32506a9-6c71-4e8d-b6a1-28d1084f12a4)

It looks like DB-0003.pem is what I need. So let’s give it a try (Figure 7).

![image](https://github.com/user-attachments/assets/090c5e70-e529-42e6-ab9b-b754f795cc14)

### 2.5.  What is the exact name of the part of the binary where the actual signature is stored?
### 2.6.  In what standard cryptographic format is the signature data stored?

The Authenticode signature in a PE file is in a PKCS #7 SignedData structure. The signature asserts that:  
• The file originates from a specific software publisher.  
• The file has not been altered since it was signed.  
Signatures can be “embedded” in a Windows PE file, in a location specified by the Certificate Table entry in Optional Header Data Directories -> Attribute Certificate Table.

PKCS7 is now Cryptographic Message Syntax(CMS).

### 2.7.  Extract the signature data from the shim binary using dd. Add 8 bytes to the location as given in the data directory to skip over the Microsoft WIN CERTIFICAT structure header (see page 14 of the specification if you are interested).

I downloaded pyew from GitHub git clone `https://github.com/joxeankoret/pyew.git`.

It is written in python 2 and needs a capstone module. To get it, I need to install pip for python 2.  
So first I downloaded it (`curl https://bootstrap.pypa.io/pip/2.7/get-pip.py --output get-pip.py`) and installed it (`sudo python2 get-pip.py`).

After that I will be able to install modules for python 2 - `sudo python2 -m pip install capstone`.  
Now I can analyze the shim (Figures 8 and 9): `sudo python2 pyew.py /boot/efi/EFI/ubuntu/shimx64.efi`.

![image](https://github.com/user-attachments/assets/c3e98bf1-1986-43a0-bbb7-fde36e2899b1)

![image](https://github.com/user-attachments/assets/0f7309b8-017e-4a08-bd57-579b52c85c1f)

Now I need to get a list of all the data directory entries (Figure 10).

![image](https://github.com/user-attachments/assets/208086f7-128d-4925-b04a-10910a34eca3)

I need to extract  
`<Structure: [IMAGE_DIRECTORY_ENTRY_SECURITY] 0x128 0x0 VirtualAddress: 0xE73C8 0x12C 0x4 Size: 0x2140>`.

To see it in a more human-readable format, I use  
`print pyew.pe.OPTIONAL_HEADER.DATA_DIRECTORY[4]`. Figure 11 shows the result.

![image](https://github.com/user-attachments/assets/2815fe0a-dbd1-45e8-b5d8-e6fe03c8a58f)



0xE73C8 - 950896. But I’m skipping the Microsoft WIN certificate structure header, so it will be 950896 + 8 = 950904.

0x2140 - 9576. But I need 9576-8 = 9568 bytes.

Now let’s extract the signature data from the shim binary using dd.

`sudo dd if=/boot/efi/EFI/ubuntu/shimx64.efi bs=1 count=9568 skip=950904 > signature`.

![image](https://github.com/user-attachments/assets/69b3493b-619b-4e9f-996e-180eb51e78bd)

Figure 12 shows the part of this file.

![image](https://github.com/user-attachments/assets/4558c3b5-5254-4cb2-b9bf-b15f4449a281)

### 2.8.  Show the subject and issuer of all X.509 certificates stored in the signature data. Draw a diagram relating these certificates to the “Microsoft Corporation UEFI CA” certificate.

I used dumpasn1 to view the data as shown in Figure 13.

![image](https://github.com/user-attachments/assets/b70410a0-266f-46fc-9819-e1fe08224e00)

So now I can compare this data with the example in the specification and find the information I need. It contains two certificates.

Here is issuer information, including sha256WithRSA signature, certificate issuer Microsoft Corporation UEFI CA 2011, organization Microsoft Corporation, country, and province (Figure 14).

![image](https://github.com/user-attachments/assets/f0ce8606-e73b-49a5-9f2b-691d380cd7d0)

Each piece of data usually has a tag variable, which corresponds to a related value.

Here is a subject with the same type of information as the issuer (Figure 15).

![image](https://github.com/user-attachments/assets/ceedd90e-b320-4ee2-9d00-540a89ec6d3c)

Figure 16 and 17 show the issuer and subject for the second certificate.

![image](https://github.com/user-attachments/assets/0ce461ff-ac79-4669-a107-50f4c29a741a)

![image](https://github.com/user-attachments/assets/3ef76378-73d2-44f6-a731-a9453b9d8f2c)


The relationship between certificates is shown in Figure 18.

![image](https://github.com/user-attachments/assets/2c5546f0-e79a-4b8a-8da5-31f8d9879305)

The shim does not contain a Microsoft root certificate.  
Microsoft Corporation UEFI CA 2011 is used by Microsoft to sign non-Microsoft UEFI bootloaders. They offer to sign EFI binaries which have passed their review with Microsoft Windows UEFI Driver Publisher key for 3rd party drivers.

## Task 3. GRUB

### 2.9.  Using your new knowledge about Authenticode binaries, extract the signing certificates from the GRUB boot loader, and show the subject and issuer.

Well, I analyzed the `grubx64.efi` file with pyew and got information about the signatures (Figure 19).

![image](https://github.com/user-attachments/assets/457712f3-cfe7-40ac-be6e-a3310da9c994)

![image](https://github.com/user-attachments/assets/5406a7e2-be04-4c55-9a39-429813afc983)

I have to skip `0x27A000 - 2596864 + 8 = 2596872` bytes and then got `0x780 - 1928 -8 = 1920` bytes, which is the signature data.

Let’s extract this as grub\_sig  
`sudo dd if=/boot/efi/EFI/ubuntu/grubx64.efi bs=1 count=1920 skip=2596872 > grub_sig`.

Then I converted hex to ASN.1 using dumpasn1. There is only one certificate. Its subject and issuer are shown in Figures 20 and 21.

![image](https://github.com/user-attachments/assets/d0ef6a20-09bc-458a-a305-9dd0cc1afb90)

![image](https://github.com/user-attachments/assets/82d69762-4e2b-47f9-8d55-191f122e8d48)

### 2.10.  Why is storing the certificate X in the shim binary secure?

As I already mentioned the signature asserts that the file has not been altered since it was signed. So if we store the certificate X in shim binary we can be sure that it will not be altered.


### 2.11.  What do you think is the subject CommonName (CN) of this X certificate?

Certificate X should be part of the chain of trust, so I think the common name of the subject would be Canonical Ltd. Master Certificate Authority.

### 2.12.  Obtain the X certificate used by the shim to verify the GRUB binary. There are two ways to obtain it: from the source code or from the binary directly. Hints for the latter case:

*   The certificate is in X.509 DER/ASN.1 format (see openssl asn1parse)
*   DER/ASN.1 leaves the CommonName readable
*   The certificate is 1080 bytes long

I’m going to get a certificate from a binary file. To do this, I downloaded OpenCorePkg and copied the shim-to-cert.tool from it. I extract the signing certificate from the shell file using the following command `sudo bash ./Utilities/ShimUtils/shim-to-cert.tool /boot/efi/EFI/ubuntu/shimx64.efi`.

As a result, I got `CanonicalLtd.MasterCertificateAuthority.pem` certificate (Figure 22) and `vendor.dbx`.

![image](https://github.com/user-attachments/assets/64e5be90-68b7-4cb0-a4d2-169d674345ba)


So my idea of the common name of the subject was correct. But the format of this certificate is pem, and its size is 1517. Let’s convert it to der: `openssl x509 -inform pem -outform der -in CanonicalLtd.MasterCertificateAuthority.pem -out cert.der`. Figure 23 shows the result.

![image](https://github.com/user-attachments/assets/367333f3-8e30-401c-8cdd-073a8b200036)

Now its size is correct. So yes, it’s an X certificate.


### 2.13.  Verify that this X certificate’s corresponding private key was indeed used to sign the GRUB binary.

To do this, I need to use dbverify as shown in Figure 24.

![image](https://github.com/user-attachments/assets/30dc2454-0e42-4602-b3e7-91669f6fc322)

Thus, the corresponding private key of certificate X was indeed used to sign the GRUB binary.

## Task 4. The-Kernel.

### 2.14.  Verify the kernel you booted against the X certificate.

the kernel is a vmlinuz file. To verify it, I need to use dbverify as shown in Figure 25.

![image](https://github.com/user-attachments/assets/916ea2f9-2f9a-4662-b0c3-0e77c7e64162)

### 2.15.  BONUS: Where does GRUB get its trusted certificate from? Hint: It is not stored in the binary, and it is not stored on the file system.

Ubuntu’s Shim includes Canonical’s public key, which validates Ubuntu’s GRUB and Linux kernel. This key is therefore stored in RAM and is rather temporary as these things go.

Here is the data directory with the certificate in the kernel in Figure 26.

![image](https://github.com/user-attachments/assets/997826c8-f3a9-410f-bc7e-b543e43d5b13)

0x9AABE0 - 14926336 + 8 = 14926344 bytes  
0x780 - 1928 - 8 = 1920 bytes

I got this using the dd command:  
`sudo dd if=/boot/efi/EFI/ubuntu/grubx64.efi bs=1 count=1920 skip=14926344 > kern_sig`.

![image](https://github.com/user-attachments/assets/09913da2-84a1-4cf0-97a6-09f3a2b6346e)

Its subject and issuer are shown in Figures 27 and 28.

![image](https://github.com/user-attachments/assets/5bf0a82c-fd17-46af-8115-6d5aaaf22d5e)

![image](https://github.com/user-attachments/assets/dd837202-8e95-4de3-9815-f03cca49589a)

This is the same as the X certificate.

Now let’s check the modules. I can see them with command `find /lib/modules/6.8.0-40-generic -name "*" | wc -l`. It shows the modules for the current kernel version. There are a lot of them - 7718.

![image](https://github.com/user-attachments/assets/3e5cedf3-aaf1-4f05-9b88-2035f50a8dcc)

I choose the following module to test - `/lib/modules/6.8.0-40-generic/misc/vboxdrv.ko`. Figure 29 shows information about this.

![image](https://github.com/user-attachments/assets/a2a3b5db-3c85-46ab-ad21-bc967b07aff9)

I extracted the module signature using [extract-module.sig.pl](http://extract-module.sig.pl) script from the kernel source (Figure 30):

`perl /usr/src/linux-headers-6.8.0-40-generic/scripts/extract-module-sig.pl -s /lib/modules/6.8.0-40-generic/misc/vboxdrv.ko > res`

![image](https://github.com/user-attachments/assets/9374029f-18b4-4bb3-a411-63fc26c9183a)

Figure 30 - The module signature

To verify this, I need a certificate and a public key from the kernel.

I already have a certificate from the shim, but I still need the public key. So I extract it from the certificate `openssl x509 -pubkey -noout -inform pem -in CanonicalLtd.MasterCertificateAuthority.pem -out pubkey` (Figure 31).

![image](https://github.com/user-attachments/assets/22afc050-2b51-4ccd-856f-e733de377ee1)
  
Figure 31 - The public key

The next step is to verify the signature using openssl.

`openssl rsautl -verify -pubin -inkey pubkey < sig > verified.bin`

OpenSSL rsautl is used to verify the encrypted signature. Here’s an explanation of the used parameters:

*   `-verify` - instruct OpenSSL rsautl to perform verification (decrypting with public key);
*   `-pubin` - the specified key file is a public key;
*   `-inkey pubkey` - the key file to use is pubkey;
*   `< sig` - read sig to the input of this command
*   `> verified.bin` redirect the output of this command to the file verified.bin

The file created by decrypting the encrypted signature will contain the message digest and associated information. But I got an error message from RSA.  Signed kernel modules have `~Module signature appended~` at the end.

### 2.16.  Draw a diagram that shows the chain of trust from the UEFI PK key to the signed kernel. Show all certificates, binaries and signing relations involved.

Figure 33 shows the chain of trust.

![Figure 33 - The chain of trust](https://i.imgur.com/ck7Rpwk.png)  

Explanations to this diagram were given in different tasks of the report, so I will not repeat.

# References

1.  [Secure Boot in ubuntu](https://wiki.ubuntu.com/UEFI/SecureBoot/Testing)
2.  [UEFI/Secure Boot](https://wiki.archlinux.org/title/Unified_Extensible_Firmware_Interface/Secure_Boot)
3.  [UEFI Specification](https://uefi.org/sites/default/files/resources/UEFI_Spec_2_9_2021_03_18.pdf)
4.  [How to verify signed UEFI binaries?](https://unix.stackexchange.com/questions/636263/how-to-verify-signed-uefi-binaries)
5.  [Windows Authenticode Portable Executable Signature Format](https://www.symbolcrash.com/wp-content/uploads/2019/02/Authenticode_PE-1.pdf)
6.  [RFC 5652](https://www.rfc-editor.org/rfc/rfc5652)
7.  [pyew](https://github.com/joxeankoret/pyew)
8.  [OpenCorePkg](https://github.com/acidanthera/OpenCorePkg/releases)
9.  [Manual verify PKCS#7 signed data with OpenSSL](http://qistoph.blogspot.com/2012/01/manual-verify-pkcs7-signed-data-with.html)
10.  [UEFI Key Management Overview](https://docs.windriver.com/bundle/Wind_River_Linux_Security_Features_Guide_CD/page/euq1494884768083.html)
11.  [ASN.1 Decoder](https://holtstrom.com/michael/tools/asn1decoder.php)
