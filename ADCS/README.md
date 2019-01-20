# 2-Tier PKI using Windows Server 2016

In this blog, I’ll guide you through the creation of 2-Tier PKI Hierarchy in Windows Server 2016 by deploying Offline Root CA and Enterprise Subordinate Issuing CA using some powershell script.

![alt text](https://vmland.files.wordpress.com/2018/11/2-pki.png?w=593)

## Installation steps:
* RootCA installation and configuration
1. Run “Install-ADCSOfflineCA.ps1” on the RootCA server

`.\Install-ADCSOfflineCA.ps1 -Company “STC Solutions” -DomainURL pki.stc.cloud -ConfigNC “cn=configuration,dc=stc,dc=cloud”`


![](https://vmland.files.wordpress.com/2018/11/screen-shot-1440-03-19-at-11-27-39-pm.png?w=593)

* 2. Confirm the installation when prompted.

![](https://vmland.files.wordpress.com/2018/11/screen-shot-1440-03-19-at-11-28-59-pm.png?w=593)

* The installation of the Root/Offline CA Role is now done.

![](https://raw.githubusercontent.com/PhilipHaglund/ADCS/master/images/3_PKI.png)

* Root CA Installation finished

![](https://raw.githubusercontent.com/PhilipHaglund/ADCS/master/images/3_PKI.png)

## JDC-CA-01 installation and configuration

1. Run “Install-ADCSSubordinateCA.ps1” on the JDC-CA-01 server
![](https://vmland.files.wordpress.com/2018/11/screen-shot-1440-03-19-at-11-45-58-pm.png?w=806&h=107)

![](https://vmland.files.wordpress.com/2018/11/screen-shot-1440-03-19-at-11-49-30-pm.png?w=593)

2. Each next setup provides a prompt that encourages a manual routine / process.

2.1 Create an Internal DNS-Zone and/or an A-record pointed to the Enterprise Subordinate CA server. It’s highly recommended to create an external publishing for the $DomainURL so the CDP is reachable from the outside.

![](https://vmland.files.wordpress.com/2018/11/screen-shot-1440-03-19-at-11-51-59-pm.png?w=593)

2.2 Sign/Issue the Enterprise/Subordinate CA Certificate on the Root/Offline CA server. *It’s recommended to not have a network connection on the Root/Offline CA Server when running in production. *

![](https://vmland.files.wordpress.com/2018/11/screen-shot-1440-03-19-at-11-55-35-pm.png?w=593)

![](https://vmland.files.wordpress.com/2018/11/screen-shot-1440-03-19-at-11-57-52-pm.png?w=593)

![](https://vmland.files.wordpress.com/2018/11/screen-shot-1440-03-19-at-11-59-03-pm.png?w=593)

![](https://vmland.files.wordpress.com/2018/11/screen-shot-1440-03-19-at-11-59-44-pm.png?w=593)

![](https://vmland.files.wordpress.com/2018/11/screen-shot-1440-03-20-at-12-00-11-am.png?w=593)


2.3 Publish a new CRL on the RootCA server.

![](https://vmland.files.wordpress.com/2018/11/screen-shot-1440-03-20-at-12-07-58-am.png?w=593)

![](https://vmland.files.wordpress.com/2018/11/screen-shot-1440-03-20-at-12-11-13-am.png?w=593)

4.4. Rename the RootCA Certificate to match the AIA location.

![](https://vmland.files.wordpress.com/2018/11/screen-shot-1440-03-20-at-12-18-25-am.png?w=593)

![](https://vmland.files.wordpress.com/2018/11/screen-shot-1440-03-20-at-12-16-14-am.png?w=593)

2.5. Copy the CRL and CRT files from the RootCA server to JDC-CA-01 server.

![](https://vmland.files.wordpress.com/2018/11/screen-shot-1440-03-20-at-12-47-02-am.png?w=593)

![](https://vmland.files.wordpress.com/2018/11/screen-shot-1440-03-20-at-12-49-01-am.png?w=593)

![](https://vmland.files.wordpress.com/2018/11/screen-shot-1440-03-20-at-12-45-00-am.png?w=593)

![](https://vmland.files.wordpress.com/2018/11/screen-shot-1440-03-20-at-12-45-08-am.png?w=593)

![](https://vmland.files.wordpress.com/2018/11/screen-shot-1440-03-20-at-12-47-20-am.png?w=593)

2.7 Automatically trying to add the RootCA certificate to the Active Directory Configuration.

![](https://vmland.files.wordpress.com/2018/11/screen-shot-1440-03-20-at-12-52-05-am.png?w=593)

![](https://vmland.files.wordpress.com/2018/11/screen-shot-1440-03-20-at-12-52-36-am.png?w=593)

2.8. Install JDC-CA-01 Certificate.

![](https://vmland.files.wordpress.com/2018/11/screen-shot-1440-03-20-at-12-52-36-am.png?w=593)

![](https://vmland.files.wordpress.com/2018/11/screen-shot-1440-03-20-at-12-52-49-am.png?w=593)

![](https://vmland.files.wordpress.com/2018/11/screen-shot-1440-03-20-at-12-53-30-am.png?w=593)

![](https://vmland.files.wordpress.com/2018/11/screen-shot-1440-03-20-at-12-53-46-am.png?w=593)

2.9. Automatically modifying “certdat.inc” file to match the Company information.

![](https://vmland.files.wordpress.com/2018/11/screen-shot-1440-03-20-at-12-56-40-am.png?w=593)

2.10 Verify the setup in pkiview.msc.

*  

* 2.10. Create a Group Policy for Certificate Auto Enrollment (Only recommended).
