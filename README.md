# Integrating a TPP with the Model Bank v3.1.11 provided by OBIE

- [Postman Collection](#Postman-Collection)</br>
- [Endpoints](#Endpoints)
    - [OpenID Connect endpoints](#OpenID-Connect-endpoints)
    - [Accounts endpoints](#Accounts-endpoints)
    - [Payment endpoints](#Payment-endpoints)
    - [Variable Recurring Payments (VRP) endpoints](#vrpendpoints)
- [Payment Debtor Test Accounts](#Payment-Debtor-Test-Accounts)
- [Financial ID](#Financial-ID)
- [Mobile Application](#Mobile-Application)
- [Step-by-step guide](#Step-by-step-guide)
    - [Step 1: Pre-Requisites (TPP)](#prereq)
    - [Step 2: Test TPP Transport Certificates (TPP)](#test_tpp)
    - [Step 3: Setup TPP On Model Bank (TPP/Open Banking)](#setup)
        - [3.1 Dynamic Client Registration (TPP)](#dcr)
            - [3.1.1 Example Registration Request JWT (Python)](#jwt)
            - [3.1.2 Request Registration Endpoint Example (Python):](#reg_endpoint)
            - [3.1.3 Basic token acquisition (optional)](#acquisition)
            - [3.1.4 Environment File download](#env_file_download)
    - [Step 4: Import Environment Files and Collections To Postman (TPP)](#import_postman)
    - [Step 5: PSU Authenticate and Authorize(Open Banking and TPP)](#psu_auth)
    - [6: Steps in  PSU Consent](#psu_consent)
    - [Step 7: Retrieve Account and Transaction Data (TPP)](#retrieve)

</br>
Open Banking (OB) offers the capability for TPPs to integrate with model banks -  Bank1 and Bank2. This model bank supports Open Banking Read/Write Standard v3.1.
</br>
</br>

# Postman Collection

This allows the various authentication and authorisation flow to be tested and some basic AISP and PISP functionality to be executed, as per the OB specifications.

[Postman](https://www.getpostman.com/) can be used to test the flows and APIs prior to building a TPP client. 

The steps below will help TPPs validate their setup and help better understand the various security flows required to integrate their own applications within the Open Banking ecosystem.

Ozone Model Bank Collection Files

[UK OBIE v3.1.11-915.postman_collection.json](./attachments/UK%20OBIE%20v3.1.11.postman_collection.json)
# Endpoints

## OpenID Connect endpoints

| Item                   | All Versions                                                                                                                                                                               |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Well-known endpoint    | [https://auth1.obie.uk.ozoneapi.io/.well-known/openid-configuration](https://auth1.obie.uk.ozoneapi.io/.well-known/openid-configuration)                                                   |
| Dynamic registration   | [https://rs1.obie.uk.ozoneapi.io/open-banking/v3.1/dynamic-client-registration/v3.1/register](https://rs1.obie.uk.ozoneapi.io/open-banking/v3.1/dynamic-client-registration/v3.1/register) |
| Token endpoint         | [https://as1.obie.uk.ozoneapi.io/token](https://as1.obie.uk.ozoneapi.io/token)                                                                                                             |
| Authorization endpoint | [https://auth1.obie.uk.ozoneapi.io/auth](https://auth1.obie.uk.ozoneapi.io/auth)                                                                                                           |

## Accounts endpoints

| Item                  |  v3.1                                                                                                                                                            |
| --------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Post-Account requests | [https://rs1.obie.uk.ozoneapi.io/open-banking/v3.1/aisp/account-access-consents](https://rs1.obie.uk.ozoneapi.io/open-banking/v3.1/aisp/account-access-consents) |
| Accounts              | [https://rs1.obie.uk.ozoneapi.io/open-banking/v3.1/aisp/accounts](https://rs1.obie.uk.ozoneapi.io/open-banking/v3.1/aisp/accounts)                               |
| Transactions          | [https://rs1.obie.uk.ozoneapi.io/open-banking/v3.1/aisp/transactions](https://rs1.obie.uk.ozoneapi.io/open-banking/v3.1/aisp/transactions)                       |
| Balances              | [https://rs1.obie.uk.ozoneapi.io/open-banking/v3.1/aisp/balances](https://rs1.obie.uk.ozoneapi.io/open-banking/v3.1/aisp/balances)                               |
| Beneficiaries         | [https://rs1.obie.uk.ozoneapi.io/open-banking/v3.1/aisp/beneficiaries](https://rs1.obie.uk.ozoneapi.io/open-banking/v3.1/aisp/beneficiaries)                     |
| Direct-Debits         | [https://rs1.obie.uk.ozoneapi.io/open-banking/v3.1/aisp/direct-debits](https://rs1.obie.uk.ozoneapi.io/open-banking/v3.1/aisp/direct-debits)                     |
| Products              | [https://rs1.obie.uk.ozoneapi.io/open-banking/v3.1/aisp/products](https://rs1.obie.uk.ozoneapi.io/open-banking/v3.1/aisp/products)                               |
| Standing-Orders       | [https://rs1.obie.uk.ozoneapi.io/open-banking/v3.1/aisp/standing-orders](https://rs1.obie.uk.ozoneapi.io/open-banking/v3.1/aisp/standing-orders)                 |

## Payment endpoints

| Item                                     | v3.1                                                                                                                                                                                               |
| ---------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Domestic Payments Consent                | [https://rs1.obie.uk.ozoneapi.io/open-banking/v3.1/pisp/domestic-payment-consents](https://rs1.obie.uk.ozoneapi.io/open-banking/v3.1/pisp/domestic-payment-consents)                               |
| Domestic Payments                        | [https://rs1.obie.uk.ozoneapi.io/open-banking/v3.1/pisp/domestic-payments](https://rs1.obie.uk.ozoneapi.io/open-banking/v3.1/pisp/domestic-payments)                                               |
| Domestic Scheduled Payments Consent      | [https://rs1.obie.uk.ozoneapi.io/open-banking/v3.1/pisp/domestic-scheduled-payment-consents](https://rs1.obie.uk.ozoneapi.io/open-banking/v3.1/pisp/domestic-scheduled-payment-consents)           |
| Domestic Scheduled Payments              | [https://rs1.obie.uk.ozoneapi.io/open-banking/v3.1/pisp/domestic-scheduled-payments](https://rs1.obie.uk.ozoneapi.io/open-banking/v3.1/pisp/domestic-scheduled-payments)                           |
| Domestic Standing Orders Consent         | [https://rs1.obie.uk.ozoneapi.io/open-banking/v3.1/pisp/domestic-standing-order-consents](https://rs1.obie.uk.ozoneapi.io/open-banking/v3.1/pisp/domestic-standing-order-consents)                 |
| Domestic Standing Orders                 | [https://rs1.obie.uk.ozoneapi.io/open-banking/v3.1/pisp/domestic-standing-orders](https://rs1.obie.uk.ozoneapi.io/open-banking/v3.1/pisp/domestic-standing-orders)                                 |
| International Payments Consent           | [https://rs1.obie.uk.ozoneapi.io/open-banking/v3.1/pisp/international-payment-consents](https://rs1.obie.uk.ozoneapi.io/open-banking/v3.1/pisp/international-payment-consents)                     |
| International Payments                   | [https://rs1.obie.uk.ozoneapi.io/open-banking/v3.1/pisp/international-payments](https://rs1.obie.uk.ozoneapi.io/open-banking/v3.1/pisp/international-payments)                                     |
| International Scheduled Payments Consent | [https://rs1.obie.uk.ozoneapi.io/open-banking/v3.1/pisp/international-scheduled-payment-consents](https://rs1.obie.uk.ozoneapi.io/open-banking/v3.1/pisp/international-scheduled-payment-consents) |
| International Scheduled Payments         | [https://rs1.obie.uk.ozoneapi.io/open-banking/v3.1/pisp/international-scheduled-payments](https://rs1.obie.uk.ozoneapi.io/open-banking/v3.1/pisp/international-scheduled-payments)                 |
| International Standing Orders Consent    | [https://rs1.obie.uk.ozoneapi.io/open-banking/v3.1/pisp/international-standing-order-consents](https://rs1.obie.uk.ozoneapi.io/open-banking/v3.1/pisp/international-standing-order-consents)       |
| International Standing Orders            | [https://rs1.obie.uk.ozoneapi.io/open-banking/v3.1/pisp/international-standing-orders](https://rs1.obie.uk.ozoneapi.io/open-banking/v3.1/pisp/international-standing-orders)                       |

## <a name="vrpendpoints"></a>Variable Recurring Payments (VRP) endpoints

| Item                 | v3.1                                                                                                                                                         |
| -------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Domestic VRP Consent | [https://rs1.obie.uk.ozoneapi.io/open-banking/v3.1/pisp/domestic-vrp-consents](https://rs1.obie.uk.ozoneapi.io/open-banking/v3.1/pisp/domestic-vrp-consents) |
| Domestic VRP         | [https://rs1.obie.uk.ozoneapi.io/open-banking/v3.1/pisp/domestic-vrps](https://rs1.obie.uk.ozoneapi.io/open-banking/v3.1/pisp/domestic-vrps)                 |


## Payment Debtor Test Accounts

<table>
<tr>
<td><b>User</b></td> <td><b>Debtor Account</b></td>
</tr>

<tr>
<td> rora </td>

<td>

```json
{
    "SchemeName" : "UK.OBIE.SortCodeAccountNumber",
    "Identification" : "70000170000001",
    "SecondaryIdentification" : "Roll No. 001",
    "Name" : "Mr. Roberto Rastapopoulos & Mr. Mitsuhirato"
}
```

</td>

</tr>

<tr>
<td> rora </td>

<td>

```json
{
    "SchemeName" : "UK.OBIE.SortCodeAccountNumber",
    "Identification" : "70000170000002",
    "SecondaryIdentification" : "Roll No. 002"
}
```

</td>

</tr>

<tr>
<td> mits </td>

<td>

```json
{
    "SchemeName" : "UK.OBIE.IBAN",
    "Identification" : "GB29OBI170000170000001",
    "Name" : "Mario Carpentry"
}
```

</td>

</tr>

<tr>
<td> rora </td>

<td>

```json
{
    "SchemeName" : "UK.OBIE.SortCodeAccountNumber",
    "Identification" : "70000170000004",
    "Name" : "Capone Consulting"
}
```

</td>

</tr>

<tr>
<td> mits </td>

<td>

```json
{
    "SchemeName" : "UK.OBIE.SortCodeAccountNumber",
    "Identification" : "70000170000005",
    "Name" : "Octon Inc"
}
```

</td>

</tr>

<tr>
<td> rora </td>

<td>

```json
{
    "SchemeName" : "UK.OBIE.SortCodeAccountNumber",
    "Identification" : "70000170000006",
    "SecondaryIdentification" : "Roll No. 002"
}
```

</td>

</tr>

</table>

## **Financial ID**

| **Item**           |
| ------------------ |
| 0015800001041RHAAY |

## Mobile Application

TPPs that want to test their app-to-app or web-to-app redirects can use the new version of Ozone Authenticator App using the instructions below:

# 1\. For Model bank users - Sandbox Mode with Mobile and Web Heimdall UI supported in parallel

The below instructions were drafted for OBL Sandbox mobile app uplift mainly for the consumption of TPPs.

1.  Install the Ozone Authenticator app from either [Play Store](https://play.google.com/store/apps/details?id=com.ozoneapi.OzoneAuthenticator) or [App Store](https://apps.apple.com/us/app/ozone-authenticator/id1592047641)
    
2.  Open the app and tap Continue.
    

<img src="./attachments/app_screen1.png" width="270" height="600"> 

3.  Click the **\+ button** on the bottom right corner to add the model bank as the provider
    

<img src="./attachments/app_screen2.png" width="270" height="600"> 

If prompted, accept the permission for the use of the camera.

<img src="./attachments/app_screen3.png" width="270" height="600"> 

4.  To add a provider, either scan the QR code or enter the code manually.
    

Scan the QR code below.

<img src="./attachments/app_screen4.png" width="270" height="270"> 

Enter provider code - “**8pozskdn**“ manually by clicking on the bottom toggle icon.

<img src="./attachments/app_screen5.png" width="270" height="600"> 

Or

Click directly on this [OBL | Sandbox](https://www.auth-app.ozoneapi.co.uk/8pozskdn?only_add_provider=true) from your mobile browser to redirect to the registration step inside the app.

5.  On being prompted for credentials enter Login Name - “**mits**” and Password - “**mits**”. Note: The credentials are case-sensitive.
    

<img src="./attachments/app_screen6.png" width="270" height="600"> 

6.  Next, you will be prompted for **biometric authentication**. This could be a fingerprint scan or face id as per your device settings. Enabling biometric authentication on your device is a mandatory prerequisite for adding the provider.
    
7.  Next, you will be prompted for **OTP Verification**. Since this is running in Sandbox mode against test data, no actual SMS will be received on your device. Enter any **6 digits** like 123456 to proceed.
    

<img src="./attachments/app_screen7.png" width="270" height="600"> 

8.  You should see a Congratulations Screen and on Continue redirected back to My Providers with the Model bank added to the list.
    

<img src="./attachments/app_screen8.png" width="270" height="600"> <img src="./attachments/app_screen9.png" width="270" height="600"> 

9.  You can view the consent list by tapping the provider row on “My Providers” screen. Further, you can also revoke the consent.
    

**Consent Journey - AIS and PIS (Single Domestic Payments)**

1.  Create a consent. Execute step 1 and 2 Post calls in the postman collection.
    
2.  Execute step 3 in the collection to get deep link. This will be in the form
    

“[https://www.auth-app.ozoneapi.co.uk/8pozskdn?client\_id=<>&response\_type=<>&scope=<>&request=](https://www.auth-app.ozoneapi.co.uk/7ob23ukz?client_id=3d8754f1-3f4f-4cd2-a7cd-c30071da4a16&response_type=code&scope=openid%20accounts&request=eyJhbGciOiJub25lIn0.eyJhdWQiOiJodHRwczovL2F1dGgxLnN0YWdpbmcub2JpZS51ay1odWIub3pvbmVhcGkuY28udWsiLCJleHAiOjE2OTI5NjUxODQsImlzcyI6IjNkODc1NGYxLTNmNGYtNGNkMi1hN2NkLWMzMDA3MWRhNGExNiIsInNjb3BlIjoib3BlbmlkIGFjY291bnRzIiwicmVkaXJlY3RfdXJpIjoiaHR0cHM6Ly93d3cuYXV0aC1hcHAub3pvbmVhcGkuY28udWsvc2ltcGxlLXJlZGlyZWN0LXVybCIsInJlc3BvbnNlX3R5cGUiOiJjb2RlIiwiY2xpZW50X2lkIjoiM2Q4NzU0ZjEtM2Y0Zi00Y2QyLWE3Y2QtYzMwMDcxZGE0YTE2Iiwibm9uY2UiOiIxYzc4NjBlOS1iZTQyLTRjNTctYjEyMS0zM2VmZTU2MmJhNWMiLCJzdGF0ZSI6ImJkN2QzNTFkLTNmZDAtNDIyNy1hNzU1LTVlYjg0NmM2YTc3NSIsIm5iZiI6MTY5Mjk2NDY4NCwibG9naW5faGludCI6InJhc3RhbWFuIiwicmVzcG9uc2VUeXBlIjoiY29kZSIsImNsYWltcyI6eyJpZF90b2tlbiI6eyJvcGVuYmFua2luZ19pbnRlbnRfaWQiOnsidmFsdWUiOiJhYWMtMWNlNjE0OWMtZjc2Yy00ODAyLThjM2MtYWVjOTkyZTU1ZTYwIiwiZXNzZW50aWFsIjp0cnVlfX19fQ.)<>”

It is important to note that if you directly copy the deep link generated from the Postman collection as a text on mobile and hit it, it will give an invalid request error due to the elimination of the last “.” in request param.

So, either send the link on your mobile as link or convert it as QR code to scan, or use visualizer.

3.  Both AIS and PIS links will require biometric authentication.
    
4.  PIS will in addition require OTP verification. Since this is running in Sandbox mode against test data, no actual SMS will be received on your device. Enter any **6 digits** like 123456 to proceed.

# Step-by-step guide

## <a name="prereq"></a>Step 1: Pre-Requisites (TPP)

Ensure that the following pre-requisites are met before onboarding onto Ozone.

1. The TPP has registered on the [Directory Sandbox](https://directory.openbanking.org.uk/s/login/)
2. The TPP has at least one software statement created on the Directory Sandbox environment
3. The TPP has at least one transport certificate created for each of its software statements.
4. The TPP has at least one redirect URI for each of its software statements.
5. The TPP has a copy of the OB root and issuing certificate attached. 

[OBSandBoxCACerts.zip](./attachments/OBSandBoxCACerts.zip)

## <a name="test_tpp"></a>Step 2: Test TPP Transport Certificates (TPP)

Testing the well know endpoint below returns a json file which contains a list of endpoints.

```
curl https://auth1.obie.uk.ozoneapi.io/.well-known/openid-configuration
```

One of these json endpoints is the token endpoint which you can use to validate your certificates.

[https://as1.obie.uk.ozoneapi.io/token](https://as1.obie.uk.ozoneapi.io/token)

The TPP should run the following check to ensure that it has a valid certificate:

``` 
curl https://as1.obie.uk.ozoneapi.io/token
-cacert ca.pem
-key {tpp-key-file}
-cert {tpp-pem-file}
```

Here, `tpp-key-file` is the file that contains the TPPs private key and `tpp-cert-file` contains the transport certificate (downloaded from Sandbox).

This will return an error response but proves that your certificates are valid against the MATLS endpoint.
Here, the `ca.pem` file contains the Open Banking issuing and root certificate chained together into a single file.

[ca.pem](./attachments/ca.pem)

## <a name="setup"></a>Step 3: Setup TPP On Model Bank (TPP/Open Banking)

Ozone banks allow TPP to onboard via dynamic client registration

### <a name="dcr"></a>3.1 Dynamic Client Registration (TPP)

See [http://openid.net/specs/openid-connect-registration-1_0-21.html](http://openid.net/specs/openid-connect-registration-1_0-21.html) 

Claims required in dynamic client registration for Ozone Banks


| Field Name                      | Example Values                                                                                                                  | Description                                                                                                                                                                                                                                                                                                                                                                                       |
| ------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| token_endpoint_auth_signing_alg | `PS256`                                                                                                                         | Signature algorithm used JWK.                                                                                                                                                                                                                                                                                                                                                                     |
| grant_types                     | authorization_code, client_credentials                                                                                          |                                                                                                                                                                                                                                                                                                                                                                                                   |
| subject_type                    | public                                                                                                                          | subject_type requested for responses to this client_id.                                                                                                                                                                                                                                                                                                                                           |
| application_type                | web                                                                                                                             | Kind of the application. The default if not specified is web. The defined values are native or web.                                                                                                                                                                                                                                                                                               |
| iss                             | software statement id                                                                                                           | The issuer must be your software ID. This is important as it's used verify it matches the SSA software ID claim.                                                                                                                                                                                                                                                                                  |
| redirect_uris                   |                                                                                                                                 | All redirect Uris should be added in the claims                                                                                                                                                                                                                                                                                                                                                   |
| token_endpoint_auth_method      | `client_secret_basic`                                                                                                           | Requested authentication method for the Token Endpoint.                                                                                                                                                                                                                                                                                                                                           |
| aud                             | `0015800001041RHAAY`                                                                                                            | The audience must match the AS issuer ID. Modelo bank's issuer ID is:<br>0015800001041RHAAY</br>                                                                                                                                                                                                                                                                                                           |
| scopes                          | <br>‘openid and accounts' or 'accounts’</br><br>‘openid and payments' or 'payments</br><br>’‘openid,accounts and payments' or 'accounts and payments’</br> | The scopes will depend on your role from the FCA (AISP and/or PISP):<ul><li>AISP: openid and accounts</li><li>PISP: openid and payments</li><li>AISP and PISP: openid, accounts and payments</li></ul>Refer to examples. Openid is optional                                                                                                                                                                                |
| request_object_signing_alg      | none                                                                                                                            |                                                                                                                                                                                                                                                                                                                                                                                                   |
| exp                             | timestamp                                                                                                                       |                                                                                                                                                                                                                                                                                                                                                                                                   |
| iat                             | timestamp                                                                                                                       |                                                                                                                                                                                                                                                                                                                                                                                                   |
| jti                             |                                                                                                                                 | UUID                                                                                                                                                                                                                                                                                                                                                                                              |
| response_types                  | code, code id_token                                                                                                             | JSON array containing a list of the OAuth 2.0 response_type values that the Client is declaring that it will restrict itself to using. If omitted, the default is that the Client will use only the code response type. See: [https://medium.com/@darutk/diagrams-of-all-the-openid-connect-flows-6968e3990660](https://medium.com/@darutk/diagrams-of-all-the-openid-connect-flows-6968e3990660) |
| id_token_signed_response_alg    | RS256                                                                                                                           | JWS algorithm                                                                                                                                                                                                                                                                                                                                                                                 |
| software_statement              |                                                                                                                                 | Since the SSA is essentially a JWT, you can decode it (for example, using [https://jwt.davetonge.co.uk/](https://jwt.davetonge.co.uk/)).                                                                                                                                                                                                                                                          |

#### <a name="jwt"></a>3.1.1 Example Registration Request JWT (Python)

##### This code is compatible with python 3.8 and requires the `jwcrypto` and `requests` modules (`pip install jwcrypto requests`)

```python
import time, uuid
from jwcrypto import jwt
from jwcrypto import jwk

# The software statement ID (software_id) of the software statement created in software statements (MIT).
SOFTWARE_STATEMENT_ID = ""

#  Value of the kid parameter associated with the signing certificate generated in Generate a
# transport/signing certificate pair (please note that you need to use the signing certificate kid).
KID = ""

# Your private signing key. You will use this to sign your JWT.
PRIVATE_RSA_KEY = """
-----BEGIN PRIVATE KEY-----
-----END PRIVATE KEY-----
"""

# Software statement assertion (SSA), generated through DFI or Directory API
SOFTWARE_STATEMENT = ""

def make_registration_jwt(software_statement_id: str, kid: str, software_statement: str) -> str:
    jwt_iat = int(time.time())
    jwt_exp = jwt_iat + 3600
    header = dict(alg='PS256', kid=kid, typ='JWT')
    claims = dict(
        token_endpoint_auth_signing_alg="PS256",
        grant_types=["authorization_code", "client_credentials"],
        subject_type="public",
        application_type="web",
        iss=software_statement_id,
        redirect_uris=["https://app.getpostman.com/oauth2/callback"], # as registered in the software statement
        token_endpoint_auth_method="client_secret_basic",
        aud="0015800001041RHAAY",
        scope= "openid accounts payments",   #accounts for AISP or payments for PISP or both
        request_object_signing_alg="none",
        exp=jwt_exp,
        iat=jwt_iat,
        jti=str(uuid.uuid4()),
        response_types=["code", "code id_token"],
        id_token_signed_response_alg="PS256",
        software_statement=software_statement
     )

    token = jwt.JWT(header=header, claims=claims)
    key_obj = jwk.JWK.from_pem(PRIVATE_RSA_KEY.encode('latin-1'))
    token.make_signed_token(key_obj)
    signed_token = token.serialize()

    print(signed_token)
    return signed_token

make_registration_jwt(SOFTWARE_STATEMENT_ID, KID, SOFTWARE_STATEMENT)


```

##### <a name="reg_endpoint">3.1.2 Request Registration Endpoint Example (Python): 

```python
import requests

headers = {'Content-Type': 'application/jwt'}
client = ('./transport.pem', './transports.key')
response = requests.post("https://rs1.obie.uk.ozoneapi.io/open-banking/v3.1/dynamic-client-registration/v3.1/register",
                         registration_request,
                         headers=headers,
                         verify=False,
                         cert=client
                        )
print(response.content)
```

The response you get from this registration is exactly the same as the one described by the OIDC dynamic registration.

You should now have successfully onboarded your TPP with the Modelo ASPSP and received a **client ID**; you will need this client ID for future interactions with the ASPSP.

##### <a name="acquisition">3.1.3 Basic token acquisition (optional)

At this stage you should be able to acquire an access token using `curl`, where values for `$CLIENT_ID` and `$CLIENT_SECRET` are provided in the response from the DCR request.

```
curl --cacert ca.pem  \
--request POST 'https://as1.obie.uk.ozoneapi.io/token' \
-u "$CLIENT_ID:$CLIENT_SECRET" \
--data-urlencode 'grant_type=client_credentials' \
--data-urlencode 'scope=accounts openid' \
--key transport.key --cert transport.pem
```

Expected result:

```json
{"access_token":"70123bf6-1111-4181-8353-4de543268d2b","token_type":"Bearer","expires_in":300}
```

##### <a name="env_file_download">3.1.4 Environment File download

Once a TPP successfully registers to Ozone, they can download the environment file for their client via an ozone helper api

Endpoint: [https://rs1.obie.uk.ozoneapi.io/ozone/v1.0/postman/environment](https://rs1.obie.uk.ozoneapi.io/ozone/v1.0/postman/environment)

Headers:

Authorization: Basic <token>

The token is the base64 encoded string of clientId: clientsecret

![Screenshot_202020-04-28_version=1&api=v2.png](./attachments/Screenshot_202020-04-28_version=1&api=v2.png)
 

On successful response, the TPP can then save the environment file to their local system.

You can accomplish the above using the following curl command:

```bash
CLIENT_ID='client_id' 
CLIENT_SECRET='client_secret'
curl --cacert ca.pem  --request GET 'https://rs1.obie.uk.ozoneapi.io/ozone/v1.0/postman/environment' \
-u "$CLIENT_ID:$CLIENT_SECRET" \
--key transport.key --cert transport.pem > $CLIENT_ID.postman_environment
```



## <a name="import_postman">Step 4: Import Environment Files and Collections To Postman (TPP)

### 4.1 Import Environment Files and Collections into Postman (TPP)

![Image 1.jpeg](./attachments/Image%201.jpeg)
 



![Image 2.png](./attachments/Image%202.png)


### 4.2 Check URLs and Environments are loaded successfully (TPP)

![Image 3.png](./attachments/Image%203.png)


### 4.3 Add Client Certificates

Add the following Modelo and Referenco bank endpoints into Postman;

**.obie.uk.ozoneapi.io:**

![image-20230720-173755.png](./attachments/image-20230720-173755.png)


The CRT should be set to the transport certificate downloaded from the open banking directory. The Key value should be set to the private key for the transport certificate.



### 4.4 SSL Certificate Verification (TPP)

**In Postman settings, ensure SSL Certificate Verification is set to off.**

## <a name="psu_auth">Step 5: PSU Authenticate and Authorize(Open Banking and TPP)

### 5.1 Client Credentials Grant (TPP)

![Image 5.png](./attachments/Image%205.png)


### 5.2 Account Access Consent(TPP)

![Image 6.jpeg](./attachments/Image%206.jpeg)


### 5.3 PSU Consent Flow(Open Banking and TPP)

TPPs can generate the consent flow URL via Postman.

![Image 7.jpeg](./attachments/Image%207.jpeg)

Once the URL is constructed, open the URL to initiate the PSU consent flow.



## <a name="psu_consent">6: Steps in  PSU Consent

### 6.1 Authenticate the user (Open Banking)

![image-20230720-174241.png](./attachments/image-20230720-174241.png)


### 6.2 Select accounts (Open Banking)

![image-20230720-174338.png](./attachments/image-20230720-174338.png)


Once the PSU consent is successful, Ozone Bank will redirect back to the redirect URI.

### 6.3 Copy the Authcode from the URL (Open Banking)



![Image 10.jpeg](./attachments/Image%2010.jpeg)


### 6.4 Generate the access token (TPP)

![Image 11.png](./attachments/Image%2011.png)


## <a name="retrieve">Step 7: Retrieve Account and Transaction Data (TPP)

### 7.1 Retrieve Account Data



![Image 12.png](./attachments/Image%2012.png)


### 7.2 Retrieve Transaction Data

![Image 13.png](./attachments/Image%2013.png)

