---
layout: default
title: CAS - Google Authenticator Authentication
---

# Google Authenticator Authentication

Google Authenticator generates 2-step verification codes on your phone. With 2-step verification signing in will require a code generated by the Google Authenticator app in addition to primary authentication. Learn more about the topic [here](https://en.wikipedia.org/wiki/Google_Authenticator).

Note that the functionality presented here should also be compatible with the likes of [LastPass Authenticator](https://lastpass.com/auth).

## Configuration

Support is enabled by including the following module in the overlay:

```xml
<dependency>
     <groupId>org.apereo.cas</groupId>
     <artifactId>cas-server-support-gauth</artifactId>
     <version>${cas.version}</version>
</dependency>
```

To see the relevant list of CAS properties, please [review this guide](Configuration-Properties.html#google-authenticator).

## Token Repository

In order to prevent reuse of tokens issued, CAS will attempt to keep track of tokens that are successfully used to authenticate the user.
The repository that holds registration records and tokens is periodically scanned and cleaned up so that expired and previously used tokens
may be removed.

## Registration

By default, an account registry implementation is included that collects user device registrations and saves them into memory.
Issued tokens are also captured into a self-cleaning cache to prevent token reuse for a configurable period of time.
This option should only be used for demo and testing purposes. Production deployments of this feature will require a separate
implementation of the registry that is capable to register accounts into persistent storage.

### JPA

Registration records and tokens may be kept inside a database instance via the following module:

```xml
<dependency>
     <groupId>org.apereo.cas</groupId>
     <artifactId>cas-server-support-gauth-jpa</artifactId>
     <version>${cas.version}</version>
</dependency>
```

To learn how to configure database drivers, [please see this guide](JDBC-Drivers.html).
To see the relevant list of CAS properties, please [review this guide](Configuration-Properties.html#google-authenticator-jpa).

### MongoDb

Registration records and tokens may be kept inside a mongo db instance, via the following module:

```xml
<dependency>
     <groupId>org.apereo.cas</groupId>
     <artifactId>cas-server-support-gauth-mongo</artifactId>
     <version>${cas.version}</version>
</dependency>
```

To see the relevant list of CAS properties, please [review this guide](Configuration-Properties.html#google-authenticator-mongodb).

### REST

Registration records may also be passed along to a REST endpoint.
The behavior is only activated when an endpoint url is provided.

| Method    | Headers             | Expected Response
|-----------|--------------------------------------------------
| `GET`     | `username`          | `200`. Secret key of the account in the body.
| `POST`    | `username`, `validationCode`, `secretKey`, `scratchCodes` | `200`. `true/false` in the body.

To see the relevant list of CAS properties, please [review this guide](Configuration-Properties.html#google-authenticator-rest).

### JSON

Registration records may also be kept inside a single JSON file for all users.
The behavior is only activated when a path to a JSON data store file is provided,
and otherwise CAS may fallback to keeping records in memory. This feature is mostly
useful during development and for demo purposes.

To see the relevant list of CAS properties, please [review this guide](Configuration-Properties.html#google-authenticator-json).
