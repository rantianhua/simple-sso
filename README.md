[![Build Status](https://travis-ci.org/samitpal/simple-sso.svg?branch=master)](https://travis-ci.org/samipal/simple-sso)

[google group](https://groups.google.com/forum/#!forum/simple-sso)

Summary
------------------
simple-sso is an SSO service with support for roles based authorization written in the Go programming language. 

For browser based applications the service exposes the /sso handler which sets the sso cookie for a given domain. For instance if the login service runs as login.example.com, the sso cookie domain could be configured as example.com. That way any application running under a subdomain of example.com will be able to leverage the sso service. The value of the sso cookie is a jwt token signed by the rsa private key of the simple-sso service. To use this service the applications need to have the corresponding public key in order to decrypt the cookie. See the code under example_app directory.

simple-sso also exposes /auth_token handler which can be used to download the encrypted jwt token. The downloaded token can potentially be passed as Authorization headers by client applications to server apps hopefully using ssl.

simple-sso also has a form of authorization capabilities. It can optionally pack in the roles (e.g ldap groups) information in the cookie/jwt based on a config environment variables..

They say a picture is thousand times more effective, so here is a diagram which shows traffic flow with simple-sso.

![alt tag](https://docs.google.com/drawings/d/1blQbqjT4lb0nu_lX-WO2OaQPvhg5I2pF0LvPZnQ9ywA/pub?w=960&h=720)

Installation
-------------------
##### To build from source follow the steps below: 

* go get github.com/jteeuwen/go-bindata

* $ go get -u github.com/samitpal/simple-sso

* $ go generate

* $ go install

Running the binary
-------------------

Following principles of 12 factor app, simple-sso uses environment variables for its configurations. These are.

|--Variable Name--|-Default--|
| sso_ssl_cert_path  |  ssl_certs/cert.pem |
| sso_ssl_key_path  |ssl_certs/key.pem   |
| sso_private_key_path  | key_pair/demo.rsa  |
| sso_weblog_dir  |  "" |
| sso_user_roles  | false  |
| sso_cookie_name  | SSO_C  |
| sso_cookie_domain  | 127.0.0.1  |
| sso_cookie_validhours  | 20  |
| sso_ldap_host  | localhost  |
| sso_ldap_port  | 389  |
| sso_ldap_ssl  | false  |
| sso_ldap_basedn  | -|
| sso_ldap_binddn  | -|
| sso_ldap_bindpasswd  | -|


Caveats
------------------
* Since time is of essence in this infrastructure, the server time needs to be set and managed correctly.
* Communication between this service and the ldap infrastruture should be encrypted.
* This has been tested with openldap.