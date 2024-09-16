![image](https://github.com/user-attachments/assets/93378c19-a64e-4783-a9ea-4fae519c0079)![image](https://github.com/user-attachments/assets/93378c19-a64e-4783-a9ea-4fae519c0079)# SSN. Lab 3: RSA

## Done by Fedorov Alexey

---

## Task 1

```
1. Create a 2048-bit RSA key pair using OpenSSL. Write your full name in
a text file and encrypt it with your private key. Using OpenSSL, extract the
public modulus and the exponent from the public key. Publish your
public key and the base64-formatted encrypted text in your report.
Verify that it is enough for decryption.
Hint: study OpenSSL params and encryption vs. verification vs decryption
vs. signing
```

Let's generate 2048 bit RSA private key (Figure 1)

```sh
openssl genrsa -des3 -out private.pem 2048
```

![Figure 1](https://github.com/user-attachments/assets/f684cd46-accd-4509-9aba-6e0dd2d47a31)

Then I create public key (Figure 2)

```sh
openssl rsa -in private.pem -outform PEM -pubout -out public.pem
```

![Figure 2](https://github.com/user-attachments/assets/300de1ae-179a-477c-8545-affa422c18af)

I've written my name to file (Figure 3)

```sh
echo "Fedorov Alexey Evgenyevich" > name.txt
```

![Figure 3](https://github.com/user-attachments/assets/dca5305a-cd8d-42fb-ba3d-88a181250885)

Let's encrypt file (Figure 4):

```sh
openssl rsautl -encrypt -inkey private.pem -in name.txt -out enc_name.txt
```

![Figure 4](https://github.com/user-attachments/assets/544c102e-eaeb-42c0-86cc-16cf79578fd7)

To extract public modulus and the exponent from the public key i used the following command (Figure 5):

```sh
openssl rsa -pubin -inform PEM -text -noout < public.pem
```

![Figure 5](https://github.com/user-attachments/assets/7e84cfa4-5f6e-4445-b243-7c80ea657dc6)

My public key is:

```
-----BEGIN PUBLIC KEY-----
MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEArzspViOOs49pp1YaHNiY
SgXgDjCZz7TMsir8p40vIYvIUf5/k+Vj3rqKCRb2UmkAB32LP9bdK4oJcVjT2WAS
Ch1KL4uImLxavP3wkRo68Tvet2nvNSSwduJsx+OZhmH2p9yz+yA6qoCty3+g7bir
mZGo47ImQjXsuybR9dKBOw1Hw2ejJIu6KOlB0euOVsRtsiRDudR76auBJnUJwk9u
TY4lCVGMhKIQWyXVH4k9GJVZQlfaALaTmTD2v1MBjiNeSb9O/oyXLAl7VIfPLv1B
L6WVoLZYOwumquPkTQ+FEGHKBL87aksOqAQC0YkbDC9WiofXusC24OEWvuvb7AhZ
iwIDAQAB
-----END PUBLIC KEY-----
```

Encrypted file base64: `cat enc_name.txt | base64`

```
dnsaILlfhDufJt6U8FvOGFisMtfSHD75eftxSqS/CpAPhBudvlBlJzQeFyAgYQYK61wcmpBIhm7f
9PPFKqMrDRhhgUKZofBgFVMBF9Fe3k4ZcQtMOrnaOrReQ0tFUlEpkcGInk6UiP01r66skvHpNYXM
KonkTChKhuUnoRbdIFD2rgjonRnBBUXccnghLNjc6SewXxb4q7u8BrSh1PJ4XjG1HxpgwkZ1n93S
BgQgNphskCtj5tFTJFXEENrDCqA+iYWBHsGrTEBdgHKf4DKSsqdBx0L+wpv1ZkLgLH7xZGmcir6J
BzLxbsPdFEt3038Mz7ibx7jHVcYjVAVN+nKing==
```

## Task 2

## Task 3

The RSA algorithm requires the selection of two large prime numbers, typically generated using a random number generator (RNG). If the RNG is predictable, it becomes easy to derive the generated keys.

Another potential vulnerability, arises when two keys share a common prime factor. In such cases, the Euclidean algorithm can be used to find the greatest common divisor, allowing the shared prime factor to be identified. By dividing each modulus by the GCD, the remaining factors can be uncovered. If two keys share one of the private prime numbers, both keys are compromised and highly vulnerable.

## Task 4

I used dcode.fr to decrypt file with keys. It was Vigenere Cipher, my number is 9.

![image](https://github.com/user-attachments/assets/6053cb1a-ac22-435b-a569-105a6005e23c)

My keys are:

```
0xbd7806710b6547bd74ae75541a5f97f25af2c2e59dddb75a87acd9ac76a1ff8db96be7d030d534277bfe47dfce69f9e6213941f916ac1ea59d6d3029919324e2c11cf20228d142ef8038c3216d63c4dc5afe03253a460f286f496d8e87a1f4625b515a472ae62516a672059ee7f0d63d8c0f4d286a75b9f7829820af8d4ce5dbL,
```
```
0xc264ffeb0ec94bfda2d428169c1162b9b687ec528a9a82dfc2ba9ff25af52f85610edf49613d07e57ea812791d8acc650157fb31138f0d32a453f031e9da29ed10a415e0757215add11999e5554fb63a21e6f52b71a4a716aabb5b34700266809a5a0e97fc276f6b9a37143704c615ef409d3d101c8f22bcde65483274b2b443L
```

Let's implement script for factor calculations. I will you python.

```
import math

hex1 = "bd7806710b6547bd74ae75541a5f97f25af2c2e59dddb75a87acd9ac76a1ff8db96be7d030d534277bfe47dfce69f9e6213941f916ac1ea59d6d3029919324e2c11cf20228d142ef8038c3216d63c4dc5afe03253a460f286f496d8e87a1f4625b515a472ae62516a672059ee7f0d63d8c0f4d286a75b9f7829820af8d4ce5db"
hex2 = "c264ffeb0ec94bfda2d428169c1162b9b687ec528a9a82dfc2ba9ff25af52f85610edf49613d07e57ea812791d8acc650157fb31138f0d32a453f031e9da29ed10a415e0757215add11999e5554fb63a21e6f52b71a4a716aabb5b34700266809a5a0e97fc276f6b9a37143704c615ef409d3d101c8f22bcde65483274b2b443"

int1 = int(hex1, 16)
int2 = int(hex2, 16)

common_factor = math.gcd(int1, int2)

print(f'Common factor: { common_factor }')

q1, reminder1 = divmod(int1, common_factor)
q2, reminder2 = divmod(int1, common_factor)

print(f"q1 : { q1 }")
print(f"q2 : { q2 }")

if not reminder1 and not reminder2:
    print("Reminders equals to zero")
```

It converts hex to int, then calculating greatest common devider, then counts other factors. Finally is checks if reminders equals to zero.

Result:

![image](https://github.com/user-attachments/assets/0f392bb2-a5da-402d-992a-f33fafd8825f)

## Task 5

