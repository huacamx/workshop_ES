# Día 2

[Cryptography on L7](https://medium.com/@16f51.4/cryptography-on-l7-f7488ad5cba1)

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


[End-to-end-user-credentials-protection](https://scotch.io/@liesware/end-to-end-user-credentials-protection)
