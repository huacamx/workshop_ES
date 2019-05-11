# Día 2

[Cryptography on L7](https://medium.com/@16f51.4/cryptography-on-l7-f7488ad5cba1)

[Algoritmos](https://github.com/liesware/coherence/wiki)

## Veracrypt

```bash
wget https://launchpad.net/veracrypt/trunk/1.23/+download/veracrypt-1.23-setup.tar.bz2
tar xjf  veracrypt-1.23-setup.tar.bz2
./veracrypt-1.23-setup-console-x64
veracrypt -t -c test.vol
mkdir vera
veracrypt test.vol vera/
df -h
veracrypt -d
```

Software adicional:

* [Acra](https://www.cossacklabs.com/acra/)
* [Vaultproject](https://www.vaultproject.io/)
* [SQLCipher](https://www.zetetic.net/sqlcipher/)

Hardware empresarial:

* [Thales](https://www.thalesesecurity.com/solutions/use-case/data-security-and-encryption/database-security)
* [Utimaco](https://hsm.utimaco.com/solutions/applications/database-encryption/)
* [Gemalto](https://safenet.gemalto.com/data-encryption/data-center-security/protect-db-database-encryption/)

## Openssl

```bash
# version
openssl version
openssl version -a

# lista de comandos
openssl help

# benchmark
openssl speed

#rand
openssl rand -hex 20

# hashes
openssl dgst -h
openssl dgst -sha1 test.vol

#HMAC
openssl dgst -sha1 -hmac "123" test.vol

# algoritmos simetricos
openssl list-cipher-commands
openssl enc -e -aes-128-cbc -in test.vol -out test.vol.enc
openssl enc -d -aes-128-cbc -in test.vol.enc -out test.vol.dec
sha1sum test.vol*

# rsa
openssl genrsa -out rsa.key 2048
openssl rsa -in rsa.key -text -noout
openssl rsa -in rsa.key -pubout -out rsa.pub
echo "1234567890123456" > password.txt
openssl rsautl -encrypt -pubin -inkey rsa.pub  -in password.txt -out password.txt.rsa
openssl rsautl -decrypt -inkey rsa.key -in password.txt.rsa -out password.txt.dec
openssl dgst -sha256 -sign rsa.key -out password.txt.sign password.txt
openssl dgst -sha256 -verify  rsa.pub -signature password.txt.sign password.txt

# dsa
openssl dsaparam 1024 < /dev/random > dsaparam.pem
openssl gendsa dsaparam.pem -out dsa.key
openssl dsa -in dsa.key -pubout -out dsa.pub
sha1sum < password.txt | awk '{print $1}' > password.txt.sha1
openssl dgst -dss1 -sign dsa.key password.txt.sha1 > password.txt.sha1.sign
openssl dgst -dss1 -verify dsa.pub -signature  password.txt.sha1.sign password.txt.sha1

#ecc
openssl ecparam -list_curves
openssl ecparam -name secp256k1 -out ecc.key
openssl ecparam -in ecc.key -text -param_enc explicit -noout

#x.509
openssl genrsa -out ca.key 2048
openssl req -new -x509 -days 365 -key ca.key -out ca.crt
openssl x509 -in ca.crt -text -noout

openssl genrsa -out server.key 2048
openssl req  -new -key server.key -out server.csr -subj  "/C=LI/ST=Liesland/L=Lies/O=Test/OU=Server/CN=localhost"
openssl x509 -req  -days 365 -in server.csr -CA ca.crt -CAkey ca.key -set_serial 01 -out server.crt
openssl x509 -in server.crt -text -noout

openssl verify -CAfile ca.crt server.crt 
openssl s_server -accept 443 -cert server.crt -key server.key 
openssl s_client -connect localhost:443 -CAfile  ca.crt

```
Lectruras adicionales:
* [RSA sign and verify using Openssl : Behind the scene](https://medium.com/@bn121rajesh/rsa-sign-and-verify-using-openssl-behind-the-scene-bf3cac0aade2)


## Coherence

* [End-to-end-user-credentials-protection](https://scotch.io/@liesware/end-to-end-user-credentials-protection)
* [PoC](https://github.com/liesware/coherence-poc)
