# Authentication

This section describes the most important aspects of authentication and authorization for an PnC EcoSystem. All Endpoints shall be secured and can only be reached after a successful authentication. 

To establish secure connections via HTTPS, Transport Layer Security via TLS 1.3 is recommended.

## Oauth2

It is recommended in using [OAuth2](https://tools.ietf.org/html/rfc6749) as authentication method. You will need to issue a [JSON Web Token (JWT)](https://tools.ietf.org/html/rfc7519) that can be obtained by different ways as described in the following sections. The token will contain information about what you are allowed to do, e.g. role (CPO, eMSP, OEM, CPS), what APIs you are allowed to use and which EMAIDs or PCIDs you are allowed to check.

The issued token will contain all necessary information to access the system (you can check this online on your own e.g. [jwt.io](https://jwt.io/)). 

## Roles&Rights

Depending on the role and the company the access rights are distinct. 

### OEM
Depending on the WMI in the PCID (first 3 characters), the OEM has read/write access to its data. Please always inform the ecosystem operator of the needed WMIs actively. If the correct data are not configured, the Plug&Charge Ecosystem will deny the access to perfom the action.

### eMSP
Depending on the EMAID (first 5 characters - CountryCode + ProviderID), the eMSP has read/write access to its data. Please always inform the ecosystem operator of the needed Country Codes and ProviderIDs activly. If the correct data are not configured, the Plug&Charge Ecosystem will deny the access to perfom the action.

|Service|Access rights                                            |Method                            |Access Rights|OEM      |CPO      |eMSP with own PKI|Mo with V2G PKI|CPS|
|-------|---------------------------------------------------------|----------------------------------|-------------|---------|---------|---------------|--------------|----|
|RCP    |OEM, CPO, eMSP                                             |getRootCert                       |Read         |Access   |Access   |Access         |Access        |Access |
|RCP    |OEM, CPO, eMSP                                             |getAllRootCerts                   |Read         |Access   |Access   |Access         |Access        |Access |
|RCP    |Admin                                                    |deleteRootCa                      |write        |No Access|No Access|No Access      |No Access     |No Access |
|RCP    |Admin                                                    |saveRootCa                        |write        |No Access|No Access|No Access      |No Access     |No Access |
|PCP    |OEM (Only those with the corresponding WMI)              |addOemProvCert                    |Write        |Access   |No Access|No Access      |No Access     |No Access |
|PCP    |OEM (Only those with the corresponding WMI)              |deleteOemProvCert                 |Delete       |Access   |No Access|No Access      |No Access     |No Access |
|PCP    |OEM,eMSP                                                   |getOEMProvCert                    |Read         |Access   |No Access|Access         |Access        |Access |
|PCP    |OEM,eMSP                                                   |lookupVehicle                     |Read         |Access   |No Access|Access         |Access        |Access |
|CPS    |OEM,eMSP,CPO,CPS                                           |getAllCpsSigners                  |(Read)       |Access   |Access   |Access         |Access        |Access |
|CPS    |eMSP with own PKI (external PKI) (only their own EMAIDS)   |contractData/signed               |(Read)       |No Access|No Access|Access         |No Access     |No Access |
|CPS    |eMSP with own PKI (external PKI) (only their own EMAIDS)   |createSignedContractData          |(Read)       |No Access|No Access|Access         |No Access     |No Access |
|CPS    |eMSP with own PKI (external PKI) (only their own EMAIDS)   |createAndForwardSignedContractData|Write        |No Access|No Access|Access         |No Access     |No Access |
|CPS    |eMSP without own PKI (V2G PKI) (only their own EMAIDS) |generateSignedContractData            |Write        |No Access|No Access|No Access      |Access        |No Access |
|CCP    |CPO                                                      |getSignedContractData             |Read         |No Access|Access   |No Access      |No Access     |No Access |
|CCP    |eMSP                                                       |deleteSignedContractData          |Delete       |No Access|No Access|Access         |Access        |No Access |
|CCP    |eMSP                                                       |lookupContract                    |Read         |No Access|No Access|Access         |Access        |No Access |
|CCP    |OEM                                                      |provideSignedContractData         |Read         |Access   |No Access|No Access      |No Access     |No Access |
|CCP    |OEM,CPS                                                  |signedContractData                |Read         |POST     |No Access|No Access      |No Access     |PUT |
|CCP    |OEM                                                      |signedContractData/priorities     |Read         |Access   |No Access|No Access      |No Access     |No Access |
|EST    |OEM                                                      |simpleEnroll - OEM                |Read & Write |Access   |No Access|No Access      |No Access     |No Access |
|EST    |OEM                                                      |caCerts - OEM                     |Read         |Access   |No Access|No Access      |No Access     |No Access |
|EST    |CPO                                                      |simpleEnroll - CPO                |Read & Write |No Access|Access   |No Access      |No Access     |No Access |
|EST    |CPO                                                      |caCerts - CPO                     |Read         |No Access|Access   |No Access      |No Access     |No Access |
|EST    |eMSP                                                       |simpleEnroll - eMSP                 |Read & Write |No Access|No Access|No Access      |Access        |No Access |
|EST    |eMSP                                                       |caCerts - eMSP                      |Read         |No Access|No Access|No Access      |Access        |No Access |
|EST    |CPS                                                      |simpleEnroll - CPS                |Read & Write |No Access|No Access|No Access      |Access        |Access |
|EST    |CPS                                                      |caCerts - CPS                     |Read         |No Access|No Access|No Access      |Access        |Access |
|WEBHOOK    |OEM, CPO, eMSP, CPS                                    |getWebhooks                       |Read         |Access|Access|Access      |Access        |Access |
|WEBHOOK    |OEM, CPO, eMSP, CPS                                    |getWebhook                        |Read         |Access|Access|Access      |Access        |Access |
|WEBHOOK    |OEM, CPO, eMSP, CPS                                    |addWebhook                        |Write         |Access|Access|Access      |Access        |Access |
|WEBHOOK    |OEM, CPO, eMSP, CPS                                    |updateWebhook                     |Write         |Access|Access|Access      |Access        |Access |
|WEBHOOK    |OEM, CPO, eMSP, CPS                                    |deleteWebhook                     |Write         |Access|Access|Access      |Access        |Access |