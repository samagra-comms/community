# Vault APIs

## 1. Overview

UCI provides an option to store the credentials required by the adapters in vault.

## 2. APIs

To add a credentials in vault, use below apis.

### 2.1 Login API

This apis provides a login token to use in the add credentials api. Below are the available parameters of api.

* **loginId:** login id of fusion auth(which is connected to vault)
* **password:** password of fusion auth(which is connected to vault)
* **applicationId:** application registered on fusion auth(which is connected to vault)

**Note**: Please [contact the administrator](../../contact-the-administrator.md) to get the **loginId** and **password** for this API.

Use below curl to get login token.

```
    curl --location --request POST 'http://auth.samagra.io:9011/api/login' \
    --header 'Authorization: YFpyHxhW0-NoKRwQrXgCU5QIAQq8nBNhE--i5_n3pTU' \
    --header 'Content-Type: application/json' \
    --header 'Cookie: refresh_token=I2RhB1AE8Ve0gpCDh4DF3ddGBvGkwuzmokIHwi-VGbZFKdo_XSTV7A' \
    --data-raw '{
        "loginId": "loginId",
        "password": "password",
        "applicationId": "a1313380-069d-4f4f-8dcb-0d0e717f6a6b"
    }'
```

This API will give below response. Use the token from the response in [Add secret/credentials API](vault-apis.md#2.2-add-a-secret-api).

```
    {
        "refreshToken": "-oYek_AgrkNGAP2cDTwgUkAsV72FDA-bRtbzmUS9Y-DOligCTx8FvA",
        "token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCIsImtpZCI6IjZoMng4Ty1IMl9XWWt5dG41ZTNuMzlwUEZlWSJ9.eyJhdWQiOiJhMTMxMzM4MC0wNjlkLTRmNGYtOGRjYi0wZDBlNzE3ZjZhNmIiLCJleHAiOjE2NTU4MTcyMDYsImlhdCI6MTY1NTgxMzYwNiwiaXNzIjoiYWNtZS5jb20iLCJzdWIiOiI4ZjdlZTg2MC0wMTYzLTQyMjktOWQyYS0wMWNlZjUzMTQ1YmEiLCJqdGkiOiI0OGY3ZjQ3ZS1lMGM4LTQ4NmItYjVlNC00NDNlZTVkMjc1NGEiLCJhdXRoZW50aWNhdGlvblR5cGUiOiJQQVNTV09SRCIsInByZWZlcnJlZF91c2VybmFtZSI6InVjaS11c2VyIiwiYXBwbGljYXRpb25JZCI6ImExMzEzMzgwLTA2OWQtNGY0Zi04ZGNiLTBkMGU3MTdmNmE2YiIsInJvbGVzIjpbIm9yZ093bmVyIl0sIm93bmVyT3JnSWQiOiJ1Y2ktdXNlciIsIm93bmVySWQiOiJ1Y2ktdXNlciJ9.GOwVGcbjVSXZpswoVtkwdf8Kv48PuNbquFPdkQgmfabUOyMwzo_n83JAq82r2x5S8lVg07ZAovBEB1D2Ak2Vfmhr_N0gxL3bdq8YrT_e3P37mVY-OPGHbSdwzIoYEdM9njyttGjue9UI_I_i4oUI7ENVWv9GFzJmwEaVjhaZiE0NHcV5gmic62cCIQo8N-lzd4lMdZEQnDS_AxYCvjhsw2P4MxXNqzkK8bhqEcMWosfLaBrO_WJ4eh_Py0HuTMeeTI0uzSEyKPUSoQJZHuTTfks_VnayqwnpSKYPSx_riY_pHjTuB8S1L52P3fFRlwQPxPcKgyIu26DVDptqNmqAXw",
        "user": {
            "active": true,
            "registrations": [
                {
                    "applicationId": "a1313380-069d-4f4f-8dcb-0d0e717f6a6b",
                    "id": "76179ff7-cd65-4c6f-9a95-d01720eacc75",
                    "roles": [
                        "orgOwner"
                    ],
                    "username": "uci-user",
                    "usernameStatus": "ACTIVE",
                    "verified": true
                }
            ],
            "username": "loginId",
            "usernameStatus": "ACTIVE",
            "verified": true
        }
    }
```

### 2.2 Add secret/credentials API

This API will add secret/credentials in the vault. Below are the available parameters of api.

* **type:** type of secret/credential **(Eg.** WhatsappGupshup/Headers)
* **secretBody:** key value pair of credentials
  *   credentials for **** type **WhatsappGupshup** should be as follows:

      ```
      "secretBody": {
              "usernameHSM": "usernameHSMValue",
              "passwordHSM": "passwordHSMValue",
              "username2Way": "username2WayValue",
              "password2Way": "password2WayValue"
      }
      ```
  *   credentials for type **Headers** should be as follows:\


      ```
      "secretBody": {
              "key1": "value1",
              "key2": "value2"
      }
      ```
* **variableName:** name of variable against which this data is being saved, it will be used in [add adapter api](bot-setup-apis.md#2.1-add-adapter) configuration. it is used to fetch secretBody data from vault when added in adapter.

Use below curl to save credentials in vault.

```
    curl --location -g --request POST 'http://143.110.183.73:3001/admin/secret' \
    --header 'ownerId: 8f7ee860-0163-4229-9d2a-01cef53145ba' \
    --header 'ownerOrgId: org1' \
    --header 'Authorization: Bearer loginToken' \
    --header 'asset: secret' \
    --header 'Content-Type: application/json' \
    --data-raw '{
        "secretBody": {
            "serviceKey": "secretKeyValue"
        },
        "type": "Headers",
        "variableName": "uci-firebase-notification"
    }'
```
