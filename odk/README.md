# ODK

## Prerequesites

Install docker and docker-compose (see [docker](../docker))

## Install

Documentation: <https://docs.getodk.org/central-install-digital-ocean/#obtaining-and-setting-up-odk-central>

Get the latest version of [ODK Central](https://github.com/getodk/central), update all submodules

```
git clone https://github.com/getodk/central
cd central
git submodule update -i
```

Copy and edit the .env file

```
mv .env.template .env
nano .env

# Use fully qualified domain names. Set to DOMAIN=local if SSL_TYPE=selfsign.
DOMAIN=your.domain.com

# Used for Let's Encrypt expiration emails and Enketo technical support emails
SYSADMIN_EMAIL=solis.admin@solidarites-liban.org

# Options: letsencrypt, customssl, upstream, selfsign
SSL_TYPE=letsencrypt

# Do not change if using SSL_TYPE=letsencrypt
HTTP_PORT=80
HTTPS_PORT=443
```

## For a local offline server

Set the SSL_TYPE to `customssl`.

### SSL Certificate

Create a Certificate Authority (CA)

```
openssl genrsa -des3 -out odk_ca.key 2048
openssl req -x509 -new -nodes -key odk_ca.key -sha256 -days 1825 -out odk_ca.pem
```

Create a private key pair for the web server

`openssl genrsa -out odk.key 2048`

Generate a Certificate Signing Request (CSR)

`openssl req -new -key odk.key -out odk.csr`

Create a .ext configuration file for the server, with the following text

```
nano odk.ext

authorityKeyIdentifier=keyid,issuer
basicConstraints=CA:FALSE
keyUsage = digitalSignature, nonRepudiation, keyEncipherment, dataEncipherment
subjectAltName = @alt_names

[alt_names]
IP.0 = 127.0.0.1
IP.1 = 192.168.88.39
IP.2 = 192.168.88.192
IP.3 = 192.168.88.193
IP.4 = 10.0.2.78
IP.5 = 192.168.88.40
IP.6 = 10.0.2.87
DNS.0 = localhost
DNS.1 = solis1.solis-demo.org
DNS.2 = solis2.solis-demo.org
DNS.3 = solis3.solis-demo.org
DNS.4 = solis4.solis-demo.org
DNS.5 = solis5.solis-demo.org
DNS.6 = solis6.solis-demo.org
```

Generate a server Certificate from the CSR + CA key pair

`openssl x509 -req -in odk.csr -CA odk_ca.pem -CAkey odk_ca.key -CAcreateserial -out odk.crt -days 1825 -sha256 -extfile odk.ext`

Copy the server certificate and server key

```
cp odk.crt central/files/local/customssl/fullchain.pem
cp odk.key central/files/local/customssl/privkey.pem
```

### Enketo

Edit the enketo.dockerfile file to add the following option

```
nano enketo.dockerfile

ENV NODE_TLS_REJECT_UNAUTHORIZED='0'
```

### ODK Collect

Get the latest version of [ODK Collect](https://github.com/getodk/collect)

`git clone https://github.com/getodk/collect`

Download and install [Android Studio](https://developer.android.com/studio/index.html)

- Copy the odk_ca.pem file to the collect_app/src/main/res/raw folder.

- Edit the `network_security_config.xml` file in `collect_app/src/main/res/xml` to add our certificate authority as a trusted source

  ```
  <?xml version="1.0" encoding="utf-8"?>
  <network-security-config>
      <base-config cleartextTrafficPermitted="true">
          <trust-anchors>
              <!-- Explicitly trust Let's Encrypt ISRG Root X1 which is not available to Android < 7.1.1 -->
              <certificates src="@raw/isrgrootx1"/>

              <!-- Explicitly trust ODK CA -->
              <certificates src="@raw/odk_ca"/>

              <certificates src="system"/>
          </trust-anchors>
      </base-config>
  </network-security-config>
  ```

- Build the APK as a signed release and install on devices.

- For phones running Android 6 and older, you also need to manually install the odk_ca and odk certificates as trusted sources.

## Start the system

Build docker images and start containers

```
docker-compose build
docker-compose up --no-start
docker-compose up -d
```

Create a new user and promote it to system administrator

```
docker-compose exec service odk-cmd --email im.techpm@solidarites-liban.org user-create
docker-compose exec service odk-cmd --email im.techpm@solidarites-liban.org user-promote
```
