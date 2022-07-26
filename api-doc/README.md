# Swiss Covid Certificate - API documentation

## :warning: The use of the API is reserved for companies operating within Switzerland. Furthermore, the API is exclusively authorized for the verification of Covid certificates within Switzerland and according to the regulations valid in Switzerland. :warning:

- [Swiss Covid Certificate - API documentation](#swiss-covid-certificate---api-documentation)
  * [Introduction](#introduction)
  * [Api docs](#api-docs)
    + [Generation, revocation and value set API doc](#generation-revocation-and-value-set-api-doc)
    + [Verification API doc](#verification-api-doc)
  * [Certificate Generation and Revocation API](#certificate-generation-and-revocation-api)
    + [HOWTO become a system integrator using the API](#howto-become-a-system-integrator-using-the-api)
    + [Third party system integration](#third-party-system-integration)
      - [Prerequisites in order to access the API](#prerequisites-in-order-to-access-the-api)
      - [Integration architecture](#integration-architecture)
        * [Integration with one-time password](#integration-with-one-time-password)
        * [Sequence diagram](#sequence-diagram)
      - [Security architecture](#security-architecture)
        * [Authorized user](#authorized-user)
        * [TLS tunnel](#tls-tunnel)
        * [Content signature](#content-signature)
          + [Java signature sample](#java-signature-sample)
          + [.NET signature sample](#net-signature-sample)
          + [Node.js TypeScript signature sample](#nodejs-typescript-signature-sample)
          + [More samples](#more-samples)
    + [Request - Certificate data](#request---certificate-data)
      - [Configuration data](#configuration-data)
      - [Personal data](#personal-data)
      - [Address data](#address-data)
      - [Transfer data - appCode](#transfer-data---appcode)
        * [Transfer code validation](#transfer-code-validation)
      - [Specific vaccination data](#specific-vaccination-data)
        * [vaccinationInfo](#vaccinationinfo)
        * [vaccination certificate data](#vaccination-certificate-data)
      - [Specific vaccination-tourist data](#specific-vaccination-tourist-data)
        * [vaccinationTouristInfo](#vaccinationtouristinfo)
        * [vaccination-tourist certificate data](#vaccination-tourist-certificate-data)
      - [Specific test data](#specific-test-data)
        * [testInfo](#testinfo)
        * [testCertificateData](#testcertificatedata)
      - [Specific recovery data](#specific-recovery-data)
        * [recoveryInfo](#recoveryinfo)
        * [recovery certificate data](#recovery-certificate-data)
      - [Specific recovery-rat data](#specific-recovery-rat-data)
        * [testInfo 2](#testInfo-2)
        * [testCertificateData 2](#testcertificatedata-2)
      - [Specific antibody data](#specific-antibody-data)
        * [antibodyInfo](#antibodyinfo)
        * [antibody certificate data](#antibody-certificate-data)
    + [Response - Covid certificate](#response---covid-certificate)
  * [Verification API](#verification-api)
  * [References](#references)
    + [Links to EU digital green certificate documentation](#links-to-eu-digital-green-certificate-documentation)

<small><i><a href='http://ecotrust-canada.github.io/markdown-toc/'>Table of contents generated with markdown-toc</a></i></small>


## Introduction

The swiss covid certificate system can be used by authorized third party systems in order to generate, revoke and verify covid certificates compatible with the EU digital green certificate. This repository contains technical information about how to integrate third party systems.

The swiss covid certificate system is hosted and maintained by the [FOITT](https://www.bit.admin.ch/bit/en/home.html).

The issuing of certificates according to type can be toggled according to the decision of the Federal Office of Public Health (FOPH).
Consequently, the API endpoints and options in the UI will be enabled/disabled. The table below shows the certificates that can currently be generated. For more information, please contact support @ covid-zertifikat@bag.admin.ch.

| Certificate type      | State |
| ----------- | ----------- |
| Vaccination      | :heavy_check_mark:       |
| Vaccination for tourists | :no_entry:        |
| Test (based on negative PCR or Rapid Antigen Test)   | :heavy_check_mark:        |
| Recovery (based on positive PCR test)   | :heavy_check_mark:        |
| Recovery (based on positive Rapid Antigen Test)   | :heavy_check_mark:        |
| Recovery (based on antibody test)   | :no_entry:        |

:heavy_check_mark:: the certificate and its corresponding endpoints are active  
:no_entry:: the certificate and its corresponding endpoints are no longer active
## Api docs

### Generation, revocation and value set API doc
  - [API File](api-docs.yaml)
  - [SwaggerUI](https://editor.swagger.io/?url=https://raw.githubusercontent.com/admin-ch/CovidCertificate-Apidoc/main/api-docs.yaml)

### Verification API doc
We do not recommend the use of the API. To save the effort of implementing the API, a Docker image of a verification service is provided. This one contains an endpoint that allows you to check, through its QR-Code, the validity of a certificate.
For mobile solutions, an Android and iOS SDK are available.

The SDKs and the Docker image of the verification service are developed and updated by our teams.
  - [CovidCertificate-SDK-Android](https://github.com/admin-ch/CovidCertificate-SDK-Android)
  - [CovidCertificate-SDK-iOS](https://github.com/admin-ch/CovidCertificate-SDK-iOS)
  - [CovidCertificate-App-Verification-Check-Service](https://github.com/admin-ch/CovidCertificate-App-Verification-Check-Service)

## Certificate Generation and Revocation API

### HOWTO become a system integrator using the API

If you are a primary system integrator, you can follow the following steps in order to use the generation and revocation API:

1. Indicate your interest to [Covid-Zertifikat@bag.admin.ch](mailto:Covid-Zertifikat@bag.admin.ch).
2. Receive a test PKI certificate of the type ["SwissGov Regular CA 01"](https://www.bit.admin.ch/bit/en/home/subsites/allgemeines-zur-swiss-government-pki/rootzertifikate/swiss-government-root-ca-ii.html) from [FOITT](https://www.bit.admin.ch/bit/en/home.html) so that the primary system integration can be developed and tested.
3. Sign an agreement with [FOITT](https://www.bit.admin.ch/bit/en/home.html). This is a condition for receiving a production PKI certificate of the type ["SwissGov Regular CA 01"](https://www.bit.admin.ch/bit/en/home/subsites/allgemeines-zur-swiss-government-pki/rootzertifikate/swiss-government-root-ca-ii.html).
4. If the test is successful and an agreement is signed, receive a production PKI certificate of the type ["SwissGov Regular CA 01"](https://www.bit.admin.ch/bit/en/home/subsites/allgemeines-zur-swiss-government-pki/rootzertifikate/swiss-government-root-ca-ii.html) from [FOITT](https://www.bit.admin.ch/bit/en/home.html) in order to generate official swiss covid certificates.
5. REST API is free of charge.
6. Batch processing is possible, but please avoid to request more than 2 covid certificates per second. In case of doubts, contact us with [Covid-Zertifikat@bag.admin.ch](mailto:Covid-Zertifikat@bag.admin.ch).

### Third party system integration

There are two methods to generate and revoke covid certificates:

1. Use the Web management UI ([prod](https://www.covidcertificate.admin.ch/) - [test](https://www.covidcertificate-a.admin.ch/)). Only authorized users determined by the cantons can use the Web management UI ([prod](https://www.covidcertificate.admin.ch/) - [test](https://www.covidcertificate-a.admin.ch/)).
2. Integrate the REST API within a primary system (system used by health professionals to manage vaccine, test and recovery information). Only authorized users determined by the cantons can use the REST API. Only primary systems determined by [FOITT](https://www.bit.admin.ch/bit/en/home.html) can access the REST API.

This documentation applies to the second use case presented above.

#### Prerequisites in order to access the API

1. Only authorized users (natural persons) can access the generation and revocation API. Authorized users are determined by the swiss cantons or [FOPH](https://www.bag.admin.ch/bag/en/home.html).
2. Third party systems have to sign an agreement with [FOITT](https://www.bit.admin.ch/bit/en/home.html) in order to access the generation and revocation API.

#### Integration architecture

##### Integration with one-time password

To use the generation and revocation API a one-time password is required that can be obtained form the Web management UI ([prod](https://www.covidcertificate.admin.ch/) - [test](https://www.covidcertificate-a.admin.ch/)).
This one-time password needs to be included in every REST API request. It is valid for 12 hours.
After expiry a new one-time password has to be generated.

![image](https://user-images.githubusercontent.com/319676/118590161-32374500-b7a2-11eb-8cb6-9395aacfa9de.png)

1. The authorized user previously registered and recognized by [eIAM](https://www.eiam.admin.ch/pages/eiam_en.html?c=eiam&l=en&ll=1) can obtain a one-time password by signing in to the Web management UI ([prod](https://www.covidcertificate.admin.ch/) - [test](https://www.covidcertificate-a.admin.ch/)) page.
2. Upon signing in to the Web management UI ([prod](https://www.covidcertificate.admin.ch/) - [test](https://www.covidcertificate-a.admin.ch/)), the authorized users rights are verified by [eIAM](https://www.eiam.admin.ch/pages/eiam_en.html?c=eiam&l=en&ll=1).
3. The authorized user must insert the one-time password in the primary system so that it is transmitted when calling the REST API.
4. One-way authentication is used to create the TLS tunnel and therefor protect the data transfer.
5. The one-time password is transferred in the requests JSON payload. [See: API doc](https://editor.swagger.io/?url=https://raw.githubusercontent.com/admin-ch/CovidCertificate-Apidoc/main/api-docs.yaml)
6. The content is hashed and signed with the primary key of the "SwissGov Regular CA 01" certificate distributed to the primary system. [See: Content signature](#content-signature).
7. The dataset structured as JSON Schema is created and transported within the secured TLS tunnel.
8. The Management Service REST API checks the integrity of the data with the received signature and verifies the the one-time password.

##### Sequence diagram

![image](https://user-images.githubusercontent.com/319676/118361751-0db64f80-b58d-11eb-8f5a-fc7e193a1a00.png)

#### Security architecture

##### Authorized user

The authorized users are onboarded in [eIAM](https://www.eiam.admin.ch/pages/eiam_en.html?c=eiam&l=en&ll=1) and can use a [CHLogin](https://www.eiam.admin.ch/?c=f!chlfaq!pub&l=en) or a [HIN](https://www.hin.ch/hin-anschluss/elektronische-identitaeten/) identity. They access the API by sending an OneTime password (OTP as [JSON Web Token - JWT](https://jwt.io/)) generated from the Web management UI ([prod](https://www.covidcertificate.admin.ch/) - [test](https://www.covidcertificate-a.admin.ch/)).

##### TLS tunnel

A TLS tunnel (single way authentication) is made between the primary system and the API gateway. One "SwissGov Regular CA 01" certificate is delivered to each primary system for this purpose.

##### Content signature

The content transferred to the REST API is signed with the private key of the certificate issued by "SwissGov Regular CA 01".

Given the JSON payload to be sent (data used to create the covid certificate or revocation data including the one-time password)), the process is as follows:

1. Primary system create a canonicalized text representation by removing all spaces, tabs, carriage returns and newlines from the payload. The regex `/[\n\r\t ]/gm` can be used.
2. Primary system encodes gets a UTF8 byte representation of that canonicalized text.
3. Primary system signs that byte representation using the algorithm "RSASSA-PKCS1-v1_5" from [RFC](https://datatracker.ietf.org/doc/html/rfc3447). Most implementations name the algorithm `SHA256withRSA`.
4. Primary system encodes the signature as base64 string.
5. Primary system places the base64 encoded signature in the request header `X-Signature`.

###### Java signature sample

```java
// load the key
PrivateKey privateKey = this.getCertificate();
// canonicalize
String normalizedJson = payload.replaceAll("[\\n\\r\\t ]", "");
byte[] bytes = normalizedJson.getBytes(StandardCharsets.UTF_8);
// sign
Signature signature = Signature.getInstance("SHA256withRSA");
signature.initSign(privateKey);
signature.update(bytes);
String signatureString = Base64.getEncoder().encodeToString(signature.sign());
```

###### .NET signature sample

```c#
// create RSA from certificate
X509Certificate2 cert = GetCertificate();
RSA rsaSignature = cert.GetRSAPrivateKey();

// normalize json
Regex normalizedJsonReplaceRegex = new Regex("[\\n\\r\\t ]");
string normalizedJson = normalizedJsonReplaceRegex.Replace(payload, string.Empty);

// sign
byte[] signatureBytes = rsaSignature.SignData(Encoding.UTF8.GetBytes(normalizedJson), HashAlgorithmName.SHA256, RSASignaturePadding.Pkcs1);

// convert signature to Base64 string
string signatureString = Convert.ToBase64String(signatureBytes);
```

###### Node.js TypeScript signature sample

```typescript
// load the key
const pemEncodedKey = fs.readFileSync(privateKeyFile)
const privateKeyObject = crypto.createPrivateKey(pemEncodedKey)
// canonicalize
const regex = /[\n\r\t ]/gm
const canonicalPayload = payload.replace(regex, '')
const bytes = Buffer.from(canonicalMessage, 'utf8')
// sign
const sign = crypto.createSign('RSA-SHA256')
sign.update(bytes)
const signature = sign.sign(privateKeyObject)
const base64encodedSignature = signature.toString('base64')
// set request header
headers['X-Signature'] = base64encodedSignature
```

###### More samples

There are samples scripts at <https://github.com/admin-ch/CovidCertificate-Api-Scripts>.

### Request - Certificate data

4 types of covid certificate can be produced: ***vaccination***, ***test***, ***recovery*** or ***antibody***. One covid certificate contains only one type.
The configuration and personal data sections are the same for all covid certificates. The other data sections are specific to the type of certificate.

One generation request generates always one single covid certificate.

#### Configuration data

Mandatory data necessary for all types of certificates:

- **otp**: the one time password which has to be generated in the [Web management UI (test environment)](https://www.covidcertificate-a.admin.ch/).
  - Format: string

#### Personal data

Mandatory data appearing in all types of certificates:

- **familyName**: family name of the covid certificate owner.
  - Format: string, maxLength: 80 CHAR.
  - Example: "Rochat"
  - Invalid chars: "!", "@", "#", "\r", "\n", "\\", "$", "%", "¶", "*", "(", ")", "_", ":", "/", "+", "=", "|", "<", ">", "?", "{", "}", "[", "]", "~"
- **givenName**: first name of the covid certificate owner.
  - Format: string, maxLength: 80 CHAR.
  - Example: "Céline"
  - Invalid chars: "!", "@", "#", "\r", "\n", "\\", "$", "%", "¶", "*", "(", ")", "_", ":", "/", "+", "=", "|", "<", ">", "?", "{", "}", "[", "]", "~"
- **dateOfBirth**: date of birth of the covid certificate owner.
  - Format: ISO 8601 date without time **or** YYYY-MM **or** YYYY
  - Examples: "1964-03-14", "1985-07", "2001"
- **language**: the national language used to create the covid certificate PDF.
  The PDF always contains English translations.
  - Accepted languages are: `de`, `it`, `fr`, `rm`.

#### Address data

Optional data for paper-based delivery of the certificate.
If this data is passed, a printout of the certificate will be sent to the specified address.
The first line of the address is derived from the personal data.
Therefore, the attributes givenName and familyName are concatenated to build this line.
Only one delivery method can be used in an API request: address data can't used together with transfer data.

- **streetAndNr**: street and house number of the recipient.  
  - Format: string.
  - Example: "Musterweg 4b"
- **zipCode**: zip code of the recipient.
  - Format: integer, maxLength: 4 CHAR, minLength: 4 CHAR.
  - Example: 3000
- **city**: city the recipient lives in.
  - Format: string.
  - Example: "Bern"
- **cantonCodeSender**: abbreviation of the canton *issuing* the certificate. This can be different from the canton the recipient of the certificiate lives in. The abbreviation is mapped to a predefined address and used as the sender of the letter when sending the printout by mail.  
  - Format: string.
  - Example: "BE"

#### Transfer data - appCode

Transfer data is used to securely deliver the covid certificate into the Covid Cert app via the InApp delivery mechanism.
Only one delivery method can be used in an API request: transfer data cannot be combined with address data.

- **appCode**: code generated by the app allowing the secure direct transfer of the covid certificate to the Covid Cert app. 
  - Format: string with 9 characters
  - Example: "Y8P8ECFN8"

In case of problem with the InApp delivery backend, the system still sets the http response status code 200 and returns the QRCode and the PDF. 
Additionally, the response contains the optional attribute **appDeliveryError**. 
If this attribut exists in the response, then the covid certificate has not been transferred to the Covid Cert app. 

**appDeliveryError** may contain the following errorCode / errorMessage combination. 

- `errorCode:476, "errorMessage": "Unknown app code."` 
  Reasons might be that the appCode is not valid any more, was already used or did not exist at all.
- `errorCode:558, "errorMessage": "App delivery failed due to a technical error."`
  Technical issue in the InApp delivery backend.
  You can try to submit the same request later. 

##### Transfer code validation

A transfer code consists of 9 characters.

The first 8 are randomly chosen from the following 29-character alphabet (code points): **`1234567890ABCDEFHKMNPRSTUWXYZ`**.
Note that the following characters are left out to reduce confusion: "0, G, I, J, L, O, Q, V".

The final 9th character is a check character (a.k.a checksum, check digit).
It is computed using the [Luhn mod N algorithm](https://en.wikipedia.org/wiki/Luhn_mod_N_algorithm).
Please use the alphabet defined above for the computation.

The following are examples of valid transfer codes:

```
Y8P8ECFN8
HDTYRB66W
YS6R7H88T
K42K6F7R2
3BY8DAZYS
ADWYF11SY
453S6HUA6
WR7UPHB4A
37WDPRSKM
01AWUUB2M
MA4S9CNUK
SY7M684WA
X216WN3YF
3C2YFKCNP
TNKBZ0TSK
```

The following codes are invalid transfer codes:

```
Y8P8ECFN9
HDTYRC66W
YS6RH788T
K43K6F7R2
3B8YDAZYS
ADWFY11SY
453S6HU6A
WR7UHPB4A
37WDRPSKM
10AWUUB2M
MAS49CNUK
SY7M864WA
$%(*(!@#$_!@*#
```

#### Specific vaccination data

##### vaccinationInfo

array containing the vaccination certificate data.
  There must be exactly one element containing the data of the latest vaccination.

##### vaccination certificate data

object containing the following fields. All fields are mandatory.

- **medicinalProductCode**: name of the medicinal product as registered in the country.
  - Format: string. Use the defined [endpoint](#generation-revocation-and-value-set-api-doc) for the value set.
    The value of the code has to be sent to the API
  - Example: "EU/1/20/1507" for a "COVID-19 Vaccine Moderna" vaccine
- **numberOfDoses**: number in a series of doses.
  - Format: integer, range: from 1 to 9.
- **totalNumberOfDoses**: total series of doses.
  - Format: integer, range: from 1 to 9.
---
**Important**
Information on the vaccine doses received (X) and required (Y) must be entered in accordance with one of the following rules:
- Last dose of a 2-dose vaccine (e.g. mRNA) without prior infection:
  - 1/2: Incomplete vaccination (not usable for travel or areas subject to certification)
  - 2/2: Full vaccination (initial immunisation)
  - 3/3, 4/4, ...: Booster
- Last dose of a 2-dose vaccine (e.g. mRNA) after prior infection (“required doses”: Y must always be 1):
  - 1/1: Full vaccination after recovery
  - 2/1, 3/1, 4/1, ...: Booster after recovery
- Last dose of a 1-dose vaccine (e.g. Janssen) without prior infection:
  - 1/1: Full vaccination
  - 2/1: Booster
---
- **vaccinationDate**: date of vaccination.
  - Format: ISO 8601 date without time.
  - Example: "2021-05-14"
- **countryOfVaccination**: the country in which the covid certificate owner has been vaccinated.
  - Format: string (2 chars according to ISO 3166 Country Codes).
  - Example: "CH" (for switzerland).

#### Specific vaccination-tourist data

##### vaccinationTouristInfo

array containing the vaccination-tourist certificate data.
  There must be exactly one element containing the data of the latest vaccination.

##### vaccination-tourist certificate data

object containing the following fields. All fields are mandatory.

- **medicinalProductCode**: name of the medicinal product as registered in the country.
  - Format: string. Use the defined [endpoint](#generation-revocation-and-value-set-api-doc) for the value set.
    The value of the code has to be sent to the API
  - Value-set: ["BBIBP-CorV", "Covaxin", "CoronaVac"]
- **numberOfDoses**: number in a series of doses.
  - Format: integer, range: from 1 to 9.
- **totalNumberOfDoses**: total series of doses.
  - Format: integer, range: from 1 to 9.
---
**Important**
Information on the vaccine doses received (X) and required (Y) must be entered in accordance with one of the following rules:
- Last dose of a 2-dose vaccine (e.g. mRNA) without prior infection:
  - 1/2: Incomplete vaccination (not usable for travel or areas subject to certification)
  - 2/2: Full vaccination (initial immunisation)
  - 3/3, 4/4, ...: Booster
- Last dose of a 2-dose vaccine (e.g. mRNA) after prior infection (“required doses”: Y must always be 1):
  - 1/1: Full vaccination after recovery
  - 2/1, 3/1, 4/1, ...: Booster after recovery
- Last dose of a 1-dose vaccine (e.g. Janssen) without prior infection:
  - 1/1: Full vaccination
  - 2/1: Booster
---
- **vaccinationDate**: date of vaccination.
  - Format: ISO 8601 date without time.
  - Example: "2021-05-14"
- **countryOfVaccination**: the country (exculding CH) in which the person has been vaccinated.
  - Format: string (2 chars according to ISO 3166 Country Codes).
  - Example: "DZ" (for Algeria).
  - Note: 'CH' is excluded from the value-set for this endpoint since the vaccination-tourist certificate is only intended for tourists (non-CH residents).

#### Specific test data

##### testInfo

array containing the test certificate data.
  There must be exactly one element containing the data of the latest test.

##### testCertificateData

object containing the following fields. All fields are mandatory if not noted otherwise.

- **typeCode**: type of test. This field is only mandatory when it is a PCR test.
  If given with manufacturerCode as well, they must match otherwise there will be a 400 BAD REQUEST.
  - Format: string.
  The value of the code has to be sent to the API
  - Accepted: "LP217198-3" for a "Rapid immunoassay" type of test
  - Accepted: "LP6464-4" for a "Nucleic acid amplification with probe detection" (PCR) type of test
- **manufacturerCode**: test manufacturer code.
  This should only be sent when it is not a PCR test, otherwise there will be a 400 BAD REQUEST.
  - Format: string.
    Use the defined [endpoint](#generation-revocation-and-value-set-api-doc) for the value set.
    The value of the code has to be sent to the API
  - Example: "1232" for a "Abbott Rapid Diagnostics, Panbio Covid-19 Ag Rapid Test" manufacturer and name
- **sampleDateTime**: date and time of the test sample collection.
  - Format: ISO 8601 date incl. time.
  - Example: "2020-09-24T17:29:41Z". "Z" means that the time is defined in the UTC timezone. "2021-07-12T16:00:00+02:00" can be used for the swiss timezone in summer.
- **testingCentreOrFacility**: name of centre or facility.
  - Format: string, maxLength: 50 CHAR.
  - Example: "Walk-in-Lyss AG"
- **memberStateOfTest**: the country in which the covid certificate owner has been tested.
  - Format: string (2 chars according to ISO 3166 Country Codes).
  - Example: "CH" (for switzerland).

#### Specific recovery data

##### recoveryInfo

array containing the recovery certificate data.
  There must be exactly one element containing the data of first positive test.

##### recovery certificate data

object containing the following fields. All fields are mandatory.

- **dateOfFirstPositiveTestResult**: date when test result was known that led to positive test obtained through a procedure established by a public health authority.
  - Format: ISO 8601 date without time.
  - Example: "2021-10-03"
- **countryOfTest**: the country in which the covid certificate owner has been tested.
  - Format: string (2 chars according to ISO 3166 Country Codes).
  - Example: "CH" (for switzerland).

#### Specific recovery-rat data

##### testInfo 2

array containing the recovery-rat certificate data.
  There must be exactly one element containing the data of the latest test.

##### testCertificateData 2

object containing the following fields. All fields are mandatory if not noted otherwise.

- **sampleDateTime**: date and time of the positive test sample collection.
  - Format: ISO 8601 date incl. time.
  - Example: "2022-01-25T17:29:41Z". "Z" means that the time is defined in the UTC timezone. 'sampleDateTime' must be greater or equal to 2021-01-10T00:00:00Z.
- **memberStateOfTest**: the country in which the covid certificate owner has been tested.
  This field must always have the value: 'CH'.
  - Format: string (2 chars according to ISO 3166 Country Codes).
  - Example: "CH" (for switzerland).

#### Specific antibody data

##### antibodyInfo

array containing the antibody certificate data.
  There must be exactly one element containing the data of the positive serology test.

##### antibody certificate data

object containing the following fields. All fields are mandatory.

- **sampleDate**: date when the sample collection for the serology test was known that led to positive test obtained through a procedure established by a public health authority.
  - Format: ISO 8601 date without time.
  - Example: "2021-10-03"
- **testingCenterOrFacility**: the ***Swissmedic*** authorization number (**mandatory**) of the laboratory + name of the laboratory (**optional**)
  - Format: string, maxLength: 50 CHAR.
  - Examples:
	  - "512345-123456789 SwissLabTest Center Zürich"
	  - "512345-123456789, SwissLabTest Center Zürich"
	  - "512345-123456789"

##### :bulb: For your information :bulb:
The Antibody Certificate specification is not part of the [EU-DCC Schema Specification](https://github.com/ehn-dcc-development/ehn-dcc-schema), this is also the reason why this certificate is only valid in Switzerland.
However, in order to ensure compatibility with the mobile applications ***COVID Certificate*** ([Android](https://play.google.com/store/apps/details?id=ch.admin.bag.covidcertificate.wallet) / [iOS](https://apps.apple.com/ch/app/covid-certificate/id1565917320)) and ***COVID Certificate Check*** ([Android](https://apps.apple.com/ch/app/covid-certificate/id1565917320) / [iOS](https://apps.apple.com/ch/app/covid-certificate-check/id1565917510)), the QR-Code of this certificate has been established upon the specification of the Test Certificate with some adaptation regarding the values of the attributes which are in back-end hardcoded:

 - Type of Test-**tt** : (from typeCode in Test Certificate request.) : [94504-8](https://loinc.org/94504-8)
 - Test Result-**tr** : [260373001](https://github.com/ehn-dcc-development/ehn-dcc-schema/blob/main/valuesets/test-result.json)
 - Date/Time of Sample Collection-**sc** : **sampleDate** at start of day (midnight: 00:00:00)

### Response - Covid certificate

The response delivered by the API contains 3 fields:

- **pdf**: the pdf encoded with base64
- **qrCode**: the tamper-proof signed QRCode as PNG image encoded with base64
- **uvci**: the unique identifier of the certificate as string.

## Verification API

You need an API token to access the verification API. Please contact us at [Covid-Zertifikat@bag.admin.ch](mailto:Covid-Zertifikat@bag.admin.ch).

## References

### Links to EU digital green certificate documentation

- [Specification of EU digital green certificate](https://ec.europa.eu/health/ehealth/covid-19_en)
- [Code repository of EU digital green certificate](https://github.com/eu-digital-green-certificates)
