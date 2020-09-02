OpenSSL
-----

- [My own CA using openssl](#My-own-CA-using-openssl)
  - [Generate CA certificate and key](#Generate-CA-certificate-and-key)
    - [Generate RSA key](#Generate-RSA-key)
    - [Generate CERT (pem file)](#Generate-CERT-pem-file)
- [Creating CA-signed Certificates](#Creating-CA-signed-Certificates)
    - [Create private key](#Create-private-key) 
    - [Create CSR](#Create-CSR)
    - [Create private key and CSR](#Create-private-key-and-CSR)
- [Sign request with CA](#Sign-request-with-CA)
    - [Create .p12 file using private key and CRT](Create-.p12-file-using-private-key-and-CRT)

## My own CA using openssl

When we need to implement security into our web apps or web services in an internal network we can implement a solution based on being our own CA to validate the requests to our servers and between them.

### Generate CA certificate and key
#### Generate RSA key
We start creating a CA certs, using the following command **genrsa** to generate an RSA private key:
```bash
openssl genrsa -des3 -out myCA.key 1024  
```
This command uses the following parameters:

 - **-des3** : the selected encryption for this key
 - **-out** : output the key to the specified file
 - **1024** : the size of the private key generate in bits

And you will be prompted for the passphrase to encrypt the key.

#### Generate CERT (pem file)
Before generating the key, we create the root certificate using the next command **req** :
```bash
openssl req -x509 -new -nodes -key myCA.key -sha256 -days 365 -out myCA.pem  
```
This command uses the following parameters:

 - **-x509** : outputs a self signed certificate instead of a certificate request. This is typically used to generate a test certificate or a self signed root CA.
 - **-new** : generates a new certificate request, if the -key option is nor used it will generate a new RSA key.
 - **-nodes** : if this option is specified then if a private key is created it will not be encrypted.
 - **-key** : file to read to private key from
 - **-[digest]** -sha256 the message digest to sign the request
 - **-days**: when the -x509 option is being used this specifies the number of days to certify the certificate for. Default is 30 days.
 - **-out** : output request to the specified file

-----
### Creating CA-signed Certificates

To sign a certificate for enable the responses over resources protected by certificates you will need to follow this steps:

#### Create private key
First, create a private key:
```bash
openssl genrsa -out client1.localhost.key 1024
```
#### Create CSR
Secondly, if have a private key you create a CSR with this command:
```bash
openssl req -new -key client1.localhost.key -out client1.localhost.csr
```

#### Create private key and CSR
In other way you can use this command to create both the private key and de certificate signing request:
```bash
openssl req -out client1.localhost.csr -new -newkey rsa:1024 -nodes -keyout client1.localhost.key
```

-----
### Sign request with CA
Then, we'll **sign** this CSR, using the CA private key and the CA cert (client1 is a folder to example valid credentials), this command uses **x509** which is a multipurpose certificate utility. It can be used to display certificate information, convert certificates to various forms, sign certificate requests like a "mini CA" or edit certificate trust settings:

```bash
openssl x509 -req -in client1/client1.localhost.csr -CA myCA.pem -CAkey myCA.key -CAcreateserial -out client1.localhost.crt -days 356 -sha256
```

This command uses the following parameters:

 - **-req** : by default a certificate is expected on input. With this option a certificate request is expected instead.
 - **-in** : this specifies the input filename to read a certificate from or standard input if this option is not specified.
 - **-CA** : specifies the CA certificate to be used for signing. When this option is present  **x509** behaves like a "mini CA". The input file is signed by this CA using this option: that is its issuer name is set to the subject name of the CA and it is digitally signed using the CAs private key.
This option is normally combined with the  **-req**  option. Without the  **-req**  option the input is a certificate which must be self signed.
 - **-CAkey** : sets the CA private key to sign a certificate with. If this option is not specified then it is assumed that the CA private key is present in the CA certificate file.
 - **-CAcreateserial**: with this option the CA serial number file is created if it does not exist: it will contain the serial number "02" and the certificate being signed will have the 1 as its serial number. Normally if the **-CA** option is specified and the serial number file does not exist it is an error.
 - **-[digest]** -sha256 the message digest to sign the request
 - **-days**: specifies the number of days to make a certificate valid for. The default is 30 days.
 - **-out** : output request to the specified file

 #### Create .p12 file using private key and CRT
 First concat private key and CRT in a new single file `p12file.txt`.
 ````bash
 cat /path/to/private.key /path/to/certificate.crt > /path/to/keyandcert.txt
 ````
 Then create the p12 file using OpenSSL
 ```bash
 openssl pkcs12 -export -in filename.txt -out filename.p12
 ```

-----
### Validations over certs and keys
#### View data on certificates, request and private keys

Check a certificate
```bash
openssl x509 -in server.crt -text -noout
```
Check a key
```bash
openssl rsa -in server.key -check
```
Check a CSR
```bash
openssl req -noout -text -verify -in server.csr
```

#### Verify a certificate and key matches

These two commands print out md5 checksums of the **certificate and key**; the checksums can be compared to verify that the certificate and key match.
```bash
openssl x509 -noout -modulus -in server.crt| openssl md5  
openssl rsa -noout -modulus -in server.key| openssl md5
```
#### Verify match between cert, CSR and key
```bash
openssl x509 -noout -modulus -in www.server.com.crt | openssl md5
openssl req -noout -modulus -in www.server.com.csr | openssl md5
openssl rsa -noout -modulus -in www.server.com.key | openssl md5
```

#### Validate whether a certificate was issued by a specific CA
To verify that a certificate is issued by a CA you can use the next command:
```bash
openssl verify -verbose -CAfile cacert.pem mycert.crt
```
and the response should be like `mycert.crt: OK`
If yo get any other message, the certificate was not issued by that CA.

-----
### Additional commands
Generate a _self-signed_ certificate (usually for test)
```bash
openssl req -x509 -newkey rsa:2048 -nodes -keyout www.server.com.key -out www.server.com.crt -days 365
```
Encrypt a private key with a passphrase
```bash
openssl rsa -in www.server.com.key -out www.server.com.key -des3
```
Remove the passphrase of an encrypted private key
```bash
openssl rsa -in www.server.com.key -out www.server.com.key
```

-----
### Create a key-pair using ssh-keygen
```bash
ssh-keygen -t rsa -b 4096 -f /path/to/key
```

This command uses the following options and will create two files, one containing the private key and other with the public key (file.pub):

 - **-t** : sets the type of the key.
 - **-b** : sets the size of the key.
 - **-f** : sets the path where the files will be created (by default into ~/.ssh folder).


#### Source:
> [Openssl docs](https://www.openssl.org/docs/man1.0.2/apps/)
