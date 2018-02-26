## Security

TLS:

The app expects a java keystore of certificates.
In order to create a new self-signed cert use:
`keytool -genkey -alias sitename -keyalg RSA -keystore keystore.jks -keysize 2048`
keystore path is configurable via:
`-Dkeystore=jmx_prometheus_httpserver/config/keystore.jks`


Connect: `https://host:port/metrics`

Authentication:

Basic HTTP auth is cofigurable via:
`-DauthConfig=jmx_prometheus_httpserver/config/jcgrealm.txt`

Make sure the file is not readable ;)

The file syntax is:
`user password role`
