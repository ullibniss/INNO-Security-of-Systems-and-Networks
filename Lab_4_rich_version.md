# SSN Lab 4. OpenVPN

## Done by Fedorov Alexey ([Telegram](https://t.me/ullibniss), [Github](https://github.com/ullibniss/))

---

## Task 2 - Set-Up

### 2.a Install, build, and initialize a Public Key Infrastructure for an installed OpenVPN.

I will follow [oficial](https://ubuntu.com/server/docs/how-to-install-and-use-openvpn) documentation to configure VPN.

Let's install openvpn & easy-rsa (Figure 1)

```sh
apt install -y openvpn easy-rsa
```
![image](https://github.com/user-attachments/assets/4a2ffbb2-281c-4dd5-9504-17b8573fffb2)

First step is to create CA directiory.

```
sudo make-cadir /etc/openvpn/easy-rsa
```

![image](https://github.com/user-attachments/assets/4da08873-7f6e-4f67-b57d-4f3fd77729a2)

Next step is pki initialization and CA building

```
 ./easyrsa init-pki
```

![image](https://github.com/user-attachments/assets/93473e07-4cb2-4327-814a-e6ec7bdfa6b3)

```
./easyrsa build-ca
```

![image](https://github.com/user-attachments/assets/50d06fb9-ecb2-46a7-9554-57482821d3e5)


### 2.b  What did you notice about the prompt when creating a CA? What is the reason behind creating and using a passphrase?

The PEM passphrase set when creating the CA will be asked for every time you need to encrypt the output of a command (such as a private key). The encryption here is important, to avoid printing any private key in plain text.

### 2.c Generate Diffie Hellman parameters and key pair for the server. Show the location(path) of the key pair generated.

Let's generate key pair for server

```
./easyrsa gen-req fedorov_alexey_server nopass
```

![image](https://github.com/user-attachments/assets/ca82a551-40e6-4f9f-a128-c3d5ca07167e)

Now I create Diffie-Hellman parameter for server

```
./easyrsa gen-dh
```

![image](https://github.com/user-attachments/assets/3aee97ad-181a-4a59-bc46-a02f79259897)

### 2.d What is the size of the Diffie Hellman parameters and their locations?

Size of the Diffie Hellman is 2048 bits

```
/etc/openvpn/easy-rsa/pki/dh.pem
```

![image](https://github.com/user-attachments/assets/3c3634c8-f3ff-429b-a883-812777c7e20a)


Documentation recommends to move files to `/etc/openvpn` :

```
cp pki/dh.pem pki/ca.crt pki/issued/myservername.crt pki/private/myservername.key /etc/openvpn/
```

![image](https://github.com/user-attachments/assets/c2aa7977-a876-4782-b2b4-e45a9491b3a7)

### 2.e Create a certificate for the server. What is the commonName value, and how many days are the certificates signed on the server?

Finally I create certificate for the server

```
./easyrsa sign-req server fedorov_alexey_server
```

![image](https://github.com/user-attachments/assets/011d54b3-4b1c-41e9-a09a-83cfe8b6a350)

Certificate signed for 825 days

**commonName**
```
commonName = fedorov_alexey_server
```
![image](https://github.com/user-attachments/assets/161503b6-a8ad-41be-a073-14c27bb26935)

### 2.f Create a key for the client and also a client certificate. What is the expiration date of the certificate?

Let's create client key pair

```
./easyrsa gen-req fedorov_alexey_client nopass
```

![image](https://github.com/user-attachments/assets/03481c2c-a280-4612-b44a-88220dcca464)

And client certificate too

```
./easyrsa sign-req client fedorov_alexey_client
```

![image](https://github.com/user-attachments/assets/e7a02b0e-a921-4e1d-953e-3459d23caf8f)

Certificate expiration date is `Dec 20 20:08:45 2026`

![image](https://github.com/user-attachments/assets/321b6e62-f88c-4ac5-916e-a578178e15cb)

### 3.a What is the TLS Authentication (TA) Key, and why is it essential in OpenVPN?

TSL Authentication key is used for `tls-auth`. The `tls-auth` directive adds an extra **HMAC** signature to all **SSL/TLS** handshake packets to verify their integrity. Any **UDP** packet without the correct **HMAC** signature is discarded. This **HMAC** signature enhances the security provided by **SSL/TLS** and offers protection against:

#### Advantages:

1. Denial-of-service (DoS) attacks or flooding on the OpenVPN UDP port.
2. Port scans aimed at identifying open server UDP ports.
3. Buffer overflow vulnerabilities within the SSL/TLS implementation.
4. Unauthorized machines attempting to initiate SSL/TLS handshakes.

It is essential for openvpn because it creates additional nessesary security level. 

### 3.b Generate TLS Authentication (server) and show the server key information



### 3.c. Generate a TLS Authentication key (client) and show the client key information
