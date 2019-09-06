# Specifications on token-related methods for the application server {#concept_54274_zh .concept}

Your application server must set parameters and obtain return values based on the following specifications when calling methods of the authorization server. Otherwise, the call may fail.

## Signature calculation {#section_oyx_yl4_hhb .section}

When the application server calls a method, for example, the token application method, the authorization server verifies the call parameters and filters invalid requests. The application server must sort and concatenate request parameters by using the specified method based on the following rules to form the to-be-signed string, and then encrypt the string by using the AccessKeySecret to obtain a signature.

**Note:** The signature calculation rules use logic that is different from the signature verification logic of the MQTT client.

The details are as follows:

-   Parameter pairs are in the format key=value.
-   Parameter pairs are separated by ampersands \(&\).
-   Parameter pairs are sorted in the alphabetic order of keys during signature calculation.
-   If a parameter pair contains multiple values of the same key, that is, same-name parameters, they are sorted in alphabetic order and connected by commas \(,\).

## Examples { .section}

Assume that the original HTTP method of a request is as follows:

```
https://mqauth.aliyuncs.com/token/apply

```

The parameters are as follows:

```
parama=a
paramc=c2,c1
paramb=b2,b1,b3

```

Sort the parameters by key as follows:

```
parama=a
paramb=b2,b1,b3
paramc=c2,c1

```

Sort the values of the same parameter in the alphabetic order. For example, sort "b2,b1,b3" as follows:

```
parama=a
paramb=b1,b2,b3
paramc=c1,c2

```

The concatenated string to be signed is as follows:

```
parama=a&paramb=b1,b2,b3&paramc=c1,c2


```

Calculate the signature by using the AccessKeySecret and the HMAC SHA-1 algorithm.

 **Note:** 

-   HTTP or HTTPS can be selected for HTTP-based token management.
-   The HTTP methods are GET and POST. POST is recommended in the production environment, and GET can be used in the test environment.

## Response status codes { .section}

The authorization server properly processes the received token request and returns a response in the JSON string format to the caller. The caller can retrieve specific values from the string as needed.

The JSON string has the following key-value mapping:

|Key|Type|Description|
|---|----|-----------|
|success|Boolean|Indicates whether the call is successful.|
|message|String|The processing result or error description.|
|code|Integer|The response code, which indicates the request processing result or error type.|
|tokenData|String|The token string, which is returned for token application.|

The following table lists the error codes.

|Code|Description|
|----|-----------|
|200|This code is returned when the call is successful.|
|400|This code is returned when an error occurs during parameter verification.|
|407|This code is returned when an error occurs during signature calculation.|
|409|This code is returned when an error occurs during token creation.|
|410|This code is returned when the token revocation failed.|
|1|This code is returned when the token is forged and cannot be parsed.|
|2|This code is returned when the token has expired.|
|3|This code is returned when the token has been revoked.|

