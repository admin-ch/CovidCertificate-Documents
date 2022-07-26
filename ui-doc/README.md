# Web Management UI documentation
Documentation for the Web management UI ([prod](
https://www.covidcertificate.admin.ch/
) - [test](
https://www.covidcertificate-a.admin.ch/
))

# Table of contents
- [Web Management UI documentation](#web-management-ui-documentation)
- [How to generate multiple COVID certificates ?](#how-to-generate-multiple-covid-certificates-)
  * [General informations](#general-informations)
  * [Certificate types](#certificate-types)
  * [Delivery methods](#delivery-methods)
  * [Templates](#templates)
    + [Supported encoding for familyName and givenName](#supported-encoding-for-familyname-and-givenname)
    + [Supported date format for all dates](#supported-date-format-for-all-dates)
    + [Supported date format for birthdates only](#supported-date-format-for-birthdates-only)
    + [Supported vaccine (vaccination certificate)](#supported-vaccine-vaccination-certificate)
    + [Supported rapid antigen tests (test certificate)](#supported-rapid-antigen-tests)
    + [Supported country](#supported-country)
  * [Troubleshooting](#troubleshooting)
- [How to revoke multiple COVID certificates ?](#how-to-revoke-multiple-covid-certificates-)
    + [Supported format for uvci](#supported-format-for-uvci)
    + [Supported values for fraud](#supported-values-for-fraud)
- [Create a CSV with Microsoft Excel](#create-a-csv-with-microsoft-excel)

# How to generate multiple COVID certificates ?
## General informations

The **Generate multiple certificates** function allows you to create and deliver several certificates by importing a CSV file. The import file can be produced using one of the available [templates](#templates).

⚠️ The CSV file supports until 100 entries and can't exceed a size of 40kB. If you have more data, we recommend to script the process with the [API](
https://github.com/admin-ch/CovidCertificate-Api-Scripts
).

## Certificate types
Four (4) types of certificate can be created with the bulk method (CSV upload):
- vaccination
- test
- recovery
- recovery-rat (rapid antigen test)

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

## Delivery methods
Three (3) types of delivery method can be used:
- sent per post
  - **only** available for **vaccination certificates**, **recovery certificates**, **recovery-rat certificates** and  **antibody certificates**
  - the certificates will be printed and sent per post
    - the patient [address](
      https://github.com/admin-ch/CovidCertificate-Apidoc#address-data
      ) is required
    - only available for addresses in Switzerland
  - certificates in PDF format will be compressed in a ZIP archive and downloaded
- transfer to the mobile app
  - available for all certificate types
  - the certificate is delivered directly in the mobile application (minimum v. 2.2.0) of the patient
  - the patient has to provide an app transfer code
    - *inAppDeliveryCode* can be generated in the mobile application (min. v. 2.2.0)
    - app tranfer code expire 30 days after generation of the code
    - only one certificate is delivered per app transfer code
  - certificates in PDF format will be compressed in a ZIP archive and downloaded
- PDF only
  - available for all certificate types
  - certificates in PDF format will be compressed in a ZIP archive and downloaded

The specifications regarding the field values of the CSV are the same as for the API. Use the [API documentation](
https://github.com/admin-ch/CovidCertificate-Apidoc#request---certificate-data
) to choose the appropriate values sets for the fields of the CSV.

## Templates
<table>
<tbody>
<tr>
<td>&nbsp;</td>
<td>&nbsp;post</td>
<td>&nbsp;appTransfer</td>
<td>&nbsp;PDF only</td>
</tr>
<tr>
<td>&nbsp;vaccination</td>
<td>&nbsp;<a href="https://github.com/admin-ch/CovidCertificate-UIdoc/blob/main/template-cc_vaccination-delivery_post.xlsx">template-cc_vaccination-delivery_post</a></td>
<td>&nbsp;<a href="https://github.com/admin-ch/CovidCertificate-UIdoc/blob/main/template-cc_vaccination-delivery_appTransfer.xlsx">template-cc_vaccination-delivery_appTransfer</a></td>
<td>&nbsp;<a href="https://github.com/admin-ch/CovidCertificate-UIdoc/blob/main/template-cc_vaccination.xlsx">template-cc_vaccination</a></td>
</tr>
<tr>
<td>&nbsp;test</td>
<td>&nbsp;not available</td>
<td>&nbsp;<a href="https://github.com/admin-ch/CovidCertificate-UIdoc/blob/main/template-cc_test-delivery_appTransfer.xlsx">template-cc_test-delivery_appTransfer</a></td>
<td>&nbsp;<a href="https://github.com/admin-ch/CovidCertificate-UIdoc/blob/main/template-cc_test.xlsx">template-cc_test</a></td>
</tr>
<tr>
<td>&nbsp;recovery</td>
<td>&nbsp;<a href="https://github.com/admin-ch/CovidCertificate-UIdoc/blob/main/template-cc_recovery-delivery_post.xlsx">template-cc_recovery-delivery_post</a></td>
<td>&nbsp;<a href="https://github.com/admin-ch/CovidCertificate-UIdoc/blob/main/template-cc_recovery-delivery_appTransfer.xlsx">template-cc_recovery-delivery_appTransfer</a></td>
<td>&nbsp;<a href="https://github.com/admin-ch/CovidCertificate-UIdoc/blob/main/template-cc_recovery.xlsx">template-cc_recovery</a></td>
</tr>
<tr>
<td>&nbsp;recovery-rat</td>
<td>&nbsp;<a href="https://github.com/admin-ch/CovidCertificate-UIdoc/blob/main/template-cc_recovery-rat-delivery_post.xlsx">template-cc_recovery-rat-delivery_post</a></td>
<td>&nbsp;<a href="https://github.com/admin-ch/CovidCertificate-UIdoc/blob/main/template-cc_recovery-rat-delivery_appTransfer.xlsx">template-cc_recovery-rat-delivery_appTransfer</a></td>
<td>&nbsp;<a href="https://github.com/admin-ch/CovidCertificate-UIdoc/blob/main/template-cc_recovery-rat.xlsx">template-cc_recovery-rat</a></td>
</tr>
</tbody>
</table>

### Supported encoding for familyName and givenName
UTF-8 | ISO-8859-1 is used for the familyName and givenName except the following characters:  
"!", "@", "#", "$", "%", "¶", "*", "(", ")", "_", ":", "/", "+", "=", "|", "<", ">", "?", "{", "}", "[", "]", "~"

### Supported date format for all dates
It is not possible to mix formats, only one of the following formats must be used in a document:
- yyyy-MM-dd (e.g. 2021-06-17)
- dd.MM.yyyy (e.g. 17.06.2021)

### Supported date format for birthdates only
It is not possible to mix formats, only one of the following formats must be used in a document:
- yyyy-MM-dd (e.g. 2021-06-17)
- dd.MM.yyyy (e.g. 17.06.2021)
- yyyy-MM (e.g. 2021-09)
- yyyy (e.g. 2021)

### Supported vaccine (vaccination certificate)
The *medicinalProductCode* has to be one one of the following code:
#### Vaccination certificate
| description (productName / productManufacturer)                                                  | medicinalProductCode         |
|--------------------------------------------------------------|--------------|
| BBIBP-CorV (Vero Cells) / China Sinopharm International Corp. - Beijing location           | **BBIBP-CorV** |
| Comirnaty / Biontech Manufacturing GmbH | **EU/1/20/1528** |
| Covaxin (also known as BBV152 A, B, C) / Bharat Biotech | **Covaxin** |
| COVID-19 Vaccine Janssen / Janssen-Cilag International    | **EU/1/20/1525** |
| COVID-19 Vaccine (Vero Cell), Inactivated/ Coronavac / Sinovac Biotech    | **CoronaVac** |
| other AstraZeneca vaccines: COVISHIELD / AZD1222 / ChAdOx1 nCoV-19/ChAdOx1-S/… / Serum Institute Of India Private Limited    | **Covishield** |
| Spikevax (previously COVID-19 Vaccine Moderna) / Moderna Biotech Spain S.L.           | **EU/1/20/1507** |
| Vaxzevria / AstraZeneca AB          | **EU/1/21/1529** |
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
### Supported rapid antigen tests
The application supports a dedicated list of rapid antigen tests. **manufacturerCode** and **typeCode** can be found [here](https://github.com/admin-ch/CovidCertificate-Examples/blob/main/cumulated/covid-19-tests_1.0.0.json) (use only those where property ch_accepted = true). Issuable tests can be found in [supported rapid antigen tests](https://corona-fachinformationen.bagapps.ch/documents/sars-cov-2-antigen-schnelltests-fachanwendung-mit-covid-zertifikat.pdf).

### Supported country
Accepted value-set: **ISO 3166-1 Alpha-2 Code**

| type                     | description|
|--------------------------|--------------------|
| vaccination              | **countryOfVaccination**: all countries |
| test                     | **memberStateOfTest**: all countries |
| recovery                 | **countryOfTest**: only Switzerland (**CH**) |
| recovery-rat             | **memberStateOfTest**: only Switzerland (**CH**) |

## Troubleshooting
If the imported CSV file can't be processed because of an error, then an error file will be sent back and no COVID certificates will be produced and delivered.
In this case, fix the errors in the CSV file according to the error description in the returned file.

# How to revoke multiple COVID certificates ?
The **Revoke multiple certificates** function allows you to revoke several certificates by importing a CSV file. The import file can be produced using the <a href="https://github.com/admin-ch/CovidCertificate-UIdoc/blob/main/template-bulk_revocation.xlsx">template-bulk_revocation.xlsx</a> template.

### Supported format for uvci
Description: UVCI is the certificate unique identifier.  
Regex: ^urn:uvci:01:CH:[A-Z0-9]{24}$  
Explanation: The **uvci** must start with 'urn:uvci:01:CH:' and is followed by 24 alpha-numeric characters (A to Z and 0 to 9)

### Supported values for fraud
Description: Determine if the certificate to be revoked has been fraudulently issued (set **true** if so).  
Values: [**true**, **false**]

# Create a CSV with Microsoft Excel

We recommend Microsoft Excel to edit the template.
1. Open the template with Microsoft Excel.
2. Fill the cells under the titled column with the respectives informations of your patients.
3. Generate the CSV:
 - Windows: ***File*** -> ***Save As*** -> ***Browse*** -> ***Save as type: CSV (Comma delimited)***
 - Mac: ***File*** -> ***Save As...*** -> ***File Format: CSV UTF-8 (Comma-delimited (.csv)***

⚠️ It is possible that when you edit the file with Excel, the date formats are modified. Please note that the date format must follow the specification. You can open the csv file with a text editor in order to check that everything is ok.
