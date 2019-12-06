# Instructions

This directory contains a sample public key `public.pem`, the corresponding
private key `private.pem`, and a message that has been encrypted with the
private key `message.bin`.

You should be able to decrypt the message using the command-line openssl
utility like this:

```bash
$openssl rsautl -inkey private.pem -decrypt < message.bin
```

If you want to see the details of the keys' contents, try:

```bash
$openssl rsa -in public.pem -pubin -text -noout
$openssl rsa -in private.pem -text -noout
```

Note that the output from OpenSSL will be in hexadecimal!

The most fundamental difference between the public and private keys is
that the public key includes the modulus n ("modulus"), while the
private key also includes the two primes p ("prime1") and q ("prime2")
such that p×q = n.

You can generate a new private key using:

```bash
$openssl genrsa -aes128 4096 > my key.pem
```

Then you can create the corresponding public key using:
```bash
$openssl rsa -in key.pem -out key-public.pem -pubout -outform PEM 
```

And finally encrypt a message with this new key using:


```bash
$openssl rsautl -encrypt -inkey key-public.pem -pubin -in mymessage.txt -out mymessage.txt.asc

```
