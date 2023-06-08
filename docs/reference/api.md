# API Documentation

## Using the API

The API for the TCI system can be found at the `/api/<version>` path, where `<version>` is the API version.
Currently, the only available version is v1.

### Authentication with the API

The API uses JWT for authentication.
To authenticate with a token, one must first be generated.
This can be done by sending the following request to the API:

```http
GET /api/v1/token HTTP/1.1
Accept: application/json
Authorization: Basic <username:password>
```

where the `<username:password>` string is again Base64 encoded.
This request returns a JSON string with the following contents:

```json
{
    'access_token': 'API_ACCESS_TOKEN',
    'access_token_expires_in': 'SECONDS_ACCESS',
    'refresh_token': 'API_REFRESH_TOKEN',
    'refresh_token_expires_in': 'SECONDS_REFRESH',
    'identity': {
        username: 'USERNAME',
        email: 'EMAIL',
        authorization_level: 'LEVEL',
    }
}
```

Once the token is obtained, the Authorization field in new HTTP requests can be modified to be the following:

```
Authorization: Bearer <token>
```

The token will be invalid after `SECONDS_ACCESS` seconds.
It can be then refreshed by sending the following request:

```http
POST /api/v1/token HTTP/1.1
Accept: application/json
Authorization: Bearer API_REFRESH_TOKEN
```

### An example request

The following example shows how to connect to the API, generate a user token, and get all the existing users.

```
GET /api/v1/token HTTP/1.1
Accept: application/json,
Authorization: Basic QWRtaW46YWRtaW4=
```
```
GET /api/v1/users HTTP/1.1
Accept: application/json
Authorization: Bearer TOKEN
```
