# openssl-templates

| License | Versioning | Build |
| ------- | ---------- | ----- |
| [![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT) | [![semantic-release](https://img.shields.io/badge/%20%20%F0%9F%93%A6%F0%9F%9A%80-semantic--release-e10079.svg)](https://github.com/semantic-release/semantic-release) | [![Build status](https://ci.appveyor.com/api/projects/status/nintdw9ns7q2sjaq/branch/master?svg=true)](https://ci.appveyor.com/project/nikAizuddin/openssl-templates/branch/master) |

OpenSSL templates.


## Example Usage (NGINX)

Create directory from the given example:
```
cp -rv certs/{example-generic,nginx}
```

``cd`` into the created directory:
```
cd certs/nginx
```

Create CA private key:
```
openssl genrsa -aes128 -out output/ca.key 2048
```

Create CA certificate:
```
openssl req -new -x509 -key output/ca.key -out output/ca.crt -config configs/ca-crt.conf -days 1825
```

Create Nginx SSL private key (*NOTE: Remove `-aes128` to create key without password*):
```
openssl genrsa -aes128 -out output/nginx.key 2048
```

Create Nginx CSR:
```
openssl req -new -key output/nginx.key -out output/nginx.csr -config configs/csr.conf
```

Prerequisites before signing CSR:
```
touch index.txt
echo '01' > serial
```

Sign CSR:
```
openssl ca -config configs/ca-policy.conf -out output/nginx.crt -infiles output/nginx.csr
```
