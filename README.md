# Swiss Covid Certificate - Documents

This repository is the landing page of the technical documentation of the swiss covid certificate. 

[API Doc](https://github.com/admin-ch/CovidCertificate-Apidoc)

[API Scripts](https://github.com/admin-ch/CovidCertificate-Api-Scripts)

[API CLI](https://github.com/admin-ch/CovidCertificate-Api-Cli)

[Web Management UI Doc](https://github.com/admin-ch/CovidCertificate-UIdoc)

[Data examples](https://github.com/admin-ch/CovidCertificate-Examples)

Technical information will be exchanged in this [mailing group](https://groups.google.com/g/covidcertificate). 

## Release notes

[Release notes](https://github.com/admin-ch/CovidCertificate-ReleaseNotes)

## Health status record

[Health status dashboard](https://status.uptrends.com/b09355b3a8714d29941af4a8d7880da9)
- Major incident - 2021-06-23 / 20h53 to 21h07: Covid certificate form not available.
- Minor incident - 2021-06-23 / 14h42 to 15h11: Covid certificate system delivered a few errors. Retry was possible.
- Minor incident - 2021-06-23 / 10h21 to 10h56: Covid certificate system delivered a few errors. Retry was possible.
- Major incident - 2021-06-20 / 09h47 to 10h18: Covid certificate system was partially not available and no covid certificate could have been created during this time. 
- Minor incident - 2021-07-02 / 08h07 to 09h24: 2 antigen tests have been wrongly deactivated and covid certificates with these tests couldn't be generated. 
- Major incident - 2021-07-29 / 18h03 to 19h35: Covid certificate system was partially not available and just some covid certificate could be created during this time. 
- Major incident - 2021-08-20 / 13h55 to 15h05: Covid certificate system was partially not available and just some covid certificate could be created during this time.
- Major incident - 2021-10-01 / 10h40 to 11h50: Covid certificate where signed with wrong keys (all certificates generated during this period are invalid), the last 10min was the service down.

## Global architecture

Here the global architecture of the COVID certificate system:

![image](https://user-images.githubusercontent.com/319676/123532187-70ae1100-d70b-11eb-8977-c581b1c8dd5c.png)


## Source code of COVID certificate system

Mobile apps:

- [CovidCertificate-App-Android](https://github.com/admin-ch/CovidCertificate-App-Android)
- [CovidCertificate-App-iOS](https://github.com/admin-ch/CovidCertificate-App-iOS)
- [CovidCertificate-SDK-Android](https://github.com/admin-ch/CovidCertificate-SDK-Android)
- [CovidCertificate-SDK-iOS](https://github.com/admin-ch/CovidCertificate-SDK-iOS)

COVID certificate services: 

- [CovidCertificate-App-Config-Service](https://github.com/admin-ch/CovidCertificate-App-Config-Service)
- [CovidCertificate-Api-Gateway-Service](https://github.com/admin-ch/CovidCertificate-Api-Gateway-Service)
- [CovidCertificate-Management-UI](https://github.com/admin-ch/CovidCertificate-Management-UI)
- [CovidCertificate-Management-Service](https://github.com/admin-ch/CovidCertificate-Management-Service)
- [CovidCertificate-Signing-Service](https://github.com/admin-ch/CovidCertificate-Signing-Service)

## Meeting documents

[20210520 - Presentation technology and integration](https://github.com/admin-ch/CovidCertificate-Documents/blob/main/20210520_CovidZertifikat_Presentation_System_Integration.pdf)

[20210527 - Presentation technology and integration - update 1](https://github.com/admin-ch/CovidCertificate-Documents/blob/main/20210527_CovidZertifikat_Presentation_System_Integration.pdf)

[20210603 - Presentation technology and integration - update 2](https://github.com/admin-ch/CovidCertificate-Documents/blob/main/20210603%20-%20Presentation%20technology%20and%20integration.pdf)

[20210610 - Presentation technology and integration - update 3](https://github.com/admin-ch/CovidCertificate-Documents/blob/main/20210610_CovidZertifikat_Presentation_System_Integration.pdf)

[20210617 - Presentation technology and integration - update 4](https://github.com/admin-ch/CovidCertificate-Documents/blob/main/20210617_CovidZertifikat_Presentation_System_Integration.pdf)

[20210624 - Presentation technology and integration - update 5](https://github.com/admin-ch/CovidCertificate-Documents/blob/main/20210624_CovidZertifikat_Presentation_System_Integration.pdf)

[20210701 - Presentation technology and integration - update 6](https://github.com/admin-ch/CovidCertificate-Documents/blob/main/20210701_CovidZertifikat_Presentation_System_Integration.pdf)

[20210826 - Presentation technology and integration - update 7](https://github.com/admin-ch/CovidCertificate-Documents/blob/main/20210826_CovidZertifikat_Presentation_System_Integration.pdf)
## References

[Information about covid certificate delivered by swiss Federal Office of Public Health (FOPH)](https://www.bag.admin.ch/covid-zertifikat)

