# ODK

## AWS Server

Create instance with :

- Ubuntu Server 16.04 LTS Amazon Machine Image (AMI)
- t2.micro
- Security group with ports open for SSH, HTTP and HTTPS
- Fixed Elastic IP associated with the instance
- known ssh key pair

## Prerequesites

1. Install docker and docker-compose (see [docker](/README/docker))
1. Disable firewall

   `sudo ufw disable`

## Install

Documentation: <https://docs.getodk.org/central-install-digital-ocean/#obtaining-and-setting-up-odk-central>

Get the latest version of [ODK Central](https://github.com/getodk/central), update all submodules

```
git clone https://github.com/getodk/central
cd central
git submodule update -i
```

Modify the .env file:

- `letsencrypt` for online server
- `customssl` for local server

```
nano .env

SSL_TYPE=selfsign|letsencrypt|customssl
DOMAIN=odk.solis-demo.org
SYSADMIN_EMAIL=solis.admin@solidarites-liban.org
```

Build docker images and start containers

```
docker-compose build
docker-compose up -d
```

Create a new user and promote it to system administrator

```
docker-compose exec service odk-cmd --email im.techpm@solidarites-liban.org user-create
docker-compose exec service odk-cmd --email im.techpm@solidarites-liban.org user-promote
```

## For local offline server

### SSL Certificate

Create a Certificate Authority (CA)

```
openssl genrsa -des3 -out my_ca.key 2048
openssl req -x509 -new -nodes -key my_ca.key -sha256 -days 1825 -out my_ca.pem
```

Create a private key pair for the web server

`openssl genrsa -out odkcentral.key 2048`

Generate a Certificate Signing Request (CSR)

`openssl req -new -key odkcentral.key -out odkcentral.csr`

Create a .ext configuration file for the server, with the following text

```
nano odkcentral.ext

authorityKeyIdentifier=keyid,issuer
basicConstraints=CA:FALSE
keyUsage = digitalSignature, nonRepudiation, keyEncipherment, dataEncipherment
subjectAltName = @alt_names

[alt_names]
IP.0 = 127.0.0.1
IP.1 = 192.168.88.192
DNS.0 = localhost
DNS.1 = odk.solis-demo.org
```

Generate a server Certificate from the CSR + CA key pair

`openssl x509 -req -in odkcentral.csr -CA my_ca.pem -CAkey my_ca.key -CAcreateserial -out odkcentral.crt -days 1825 -sha256 -extfile odkcentral.ext`

Copy server certificate and server key

```
cp odkcentral.crt <odk central path>/files/local/customssl/fullchain.pem
cp odkcentral.key <odk central path>/files/local/customssl/privkey.pem
```

### ODK Collect

Get the latest version of [ODK Collect](https://github.com/getodk/collect)

`git clone https://github.com/getodk/collect`

Download and install [Android Studio](https://developer.android.com/studio/index.html)

Add a network security configuration to Android Manifest:

- add in `collect_app/src/main/AndroidManifest.xml`

  ```
    <application
        android:name=".application.Collect"
        [...]
        android:networkSecurityConfig="@xml/network_security_config">
  ```

- create a `network_security_config.xml` file in `collect_app/src/main/res/xml`

  ```
  <?xml version="1.0" encoding="utf-8"?>
  <network-security-config>
      <base-config>
          <trust-anchors>
              <certificates src="@raw/my_ca"/>
              <certificates src="system"/>
          </trust-anchors>
      </base-config>
  </network-security-config>
  ```

- Paste the my_ca.pem file to a collect_app/src/main/res/raw folder.

Build the APK in release and install on devices.
