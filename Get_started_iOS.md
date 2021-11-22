# Kasko2go Open source for iOS

Open source solution SDK for iOS description consists of the following components:
- Steps to start an Open source project;
- Steps fоr integrating the Open source into your solutions;
- Description of interaction with the Open source solution APIs to obtain scoring data;
- Onboarding and main screens for iOS.

<br/>

## Steps to start an Open source project

To work with the Open source project you need to receive the access to our repositories on [Github][git] that contain source codes and technical documentation of the project.
Provide our [Technical Support Service][TSS] with the information about your Github user account ([Github][git] e-mail address) and receive the following access parameters:
- USER - account username;
- PASSWORD - user account password;
- NAVIGATOR_URL - navigation service address URL. 



<br/>

### Telematic data

1. Find the `APIRequestManager` file in the source code repository. Add the lines with access parameters received from out Technical Support Service to the end of the `APIRequestManager` file:
```
prodURL = “https://scoring-api.kasko2go.net/”
2username = “<user_name>”
3password = “<user_password>”
4tripsURL = “<https://navigation_url.host.com/>”
```

2. Find the `DataSender` file in the source code repository. Add the lines with parameters received from out [Technical Support Service][TSS] to the end of the `DataSender` file:
```
sendHost = “receiver.kasko2go.net”
```


### Cartography service
To work with Google autocomplete service receive an **API key for the Maps SDK for iOS**. The procedure for receiving the key is described in [Using API Keys][UAPIK_iOS].
Specify the received Google **API key for the Maps SDK for iOS** in `SearchAddressViewController` file in the following form:
```
override func viewDidLoad() {
    super.viewDidLoad()
 GMSPlacesClient.provideAPIKey(“<api_key>“) 
}
```

### Firebase services

Provide interaction of your iOS application with Firebase services. For this add the `GoogleService-Info.plist` configuration file to your mobile application. Follow [this link for][FB_iOS] the procedure of adding the `GoogleService-Info.plist` configuration file to your mobile application. 

<br/>

## Steps fоr integrating the Open source into your solutions









[git]: <https://github.com/>
 [TSS]: <mailto:info@kasko2go.com>
 [UAPIK_iOS]: <https://developers.google.com/maps/documentation/ios-sdk/get-api-key>
 [FB_iOS]: <https://developers.google.com/maps/documentation/ios-sdk/get-api-key>
 [SAA]: <>

