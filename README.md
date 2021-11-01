# Kasko2go Open source telematic app for Android


## _Description_
The **Kasko2go Open source** service is developed to determine a driving behaviour of a certain driver. This telematic service works with digitised road maps, has the information on static and dynamic  accident rate of different road sections.
A driver’s smartphone is used as a client telematic device. An accelerometer and a gyroscope, modules of GPS, GSM and Bluetooth systems embedded into a smartphone turn a driver’s smartphone into a source of telematic information. A mobile application installed on the smartphone transmits telematic data to servers for further processing.

## _Application Functionality:_



| Finding the “optimal route” between the start and end points of the trip  | Provides information about route safety |
| :-------------: | :-------------: |
| ![Image text](./Pictures/5.4.jpg)  | ![Image text](./Pictures/5.5a.jpg)  |
| Evaluation of risk (probability) of causing an accident by a driver | Collection and primary processing of telemetry data on vehicle movement |
| ![Image text](./Pictures/2.2.jpg)  | ![Image text](./Pictures/3.1.jpg)  |

<br/>

This free of charge app can be used to create your own app for Android.

## Get started

To work with the Open source project you need to receive the access to our repositories on [Github][git] that contain source codes and technical documentation of the project.
Provide our [Technical Support Service][TSS] with the information about your Github user account ([Github][git] e-mail address) and receive the following access parameters:
- USER - account username;
- PASSWORD - user account password;
- NAVIGATOR_URL - navigation service address URL. 


<br/>

## Telematic data

1. Find the `sdk-telemetry/keystore.properties` file in the form given below in the source code repository:
```
1 WEB_API="https://scoring-api.kasko2go.net/api/"
2 RECEIVER="receiver.kasko2go.net"
```

2. Add the lines with access parameters received from out [Technical Support Service][TSS] to the end of the `sdk-telemetry/keystore.properties` file on Step 1, then this file takes the following form: 

```
1 WEB_API="https://scoring-api.kasko2go.net/api/"
2 RECEIVER="receiver.kasko2go.net"
3
4 USER="<user_name>"
5 PASSWORD="<user_password>"
6 NAVIGATOR_URL="<https://navigation_url.host.com/>"
```


## Cartography service
1. To work with Google autocomplete service receive an **API key for the Maps SDK for Android**. The procedure for receiving the key is described in [Using API Keys][UAPIK].
Specify the received Google **API key for the Maps SDK for Android** in `sdk-telemetry/keystore.properties` file, which takes the following form:
```
1 WEB_API="https://scoring-api.kasko2go.net/api/"
2 RECEIVER="receiver.kasko2go.net"
3
4 GOOGLE_API_KEY="<api_key>"
5 USER="<user_name>"
6 PASSWORD="<user_password>"
7 NAVIGATOR_URL="<https://navigation_url.host.com/>"
```

2. To work with Google maps services receive a **Google Maps API key**. The procedure for receiving the key is described in [Maps SDK for Android Quickstart][MSDK].
Specify the received Google **Maps API key** in `app/src/main/AndroidManifest.xml` file in the following form:
```
1 <meta-data
2    android:name="com.google.android.geo.API_KEY"
3    android:value="<key>" />
```



## Firebase services

Provide interaction of your Android application with Firebase services. For this add the `google-services.json` configuration file to your mobile application. Follow [this link for][FB] the procedure of adding the `google-services.json` configuration file to your mobile application. 


## Initialization Android SDK

To integrate the Open source solution SDK into your new project, you need to perform the following steps after contacting our [Technical Support Service][TSS]: 
1. Add necessary permissions <br/>
Add necessary permissions to your new project and ask a smartphone user to allow these permissions:
- Internet
- application operation in the background 
- geolocation service
- physical activity
- disabling power-saving mode
- enabling Bluetooth adapter
- you must declare a dependency to the Google Location and Activity Recognition API version 12.0.0 or higher 

```
1    <uses-permission android:name="android.permission.INTERNET" />
2    <uses-permission android:name="android.permission.ACCESS_BACKGROUND_LOCATION" />
3    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
4    <uses-permission android:name="android.permission.ACTIVITY_RECOGNITION" />
5    <uses-permission android:name="android.permission.REQUEST_IGNORE_BATTERY_OPTIMIZATIONS" />
6    <uses-permission android:name="android.permission.BLUETOOTH" />
7    <uses-permission android:name="com.google.android.gms.permission.ACTIVITY_RECOGNITION" />
 ```


2. Implement metadata <br/>
Metadata contains extra information about a user, a device and a vehicle:
- userId - unique identifier of the user
- deviceId - unique identifier of the device
- vehicleId - unique identifier of the vehicle
These identifiers allow to uniquely identify a user, are randomly generated by the mobile application, must be unique and have a constant value for each user and user device. If the mobile application has been reinstalled, these identifiers are generated again.  
<br/>
The identifiers userId, deviceId, vehicleId are described by the following class:

```
1 class Metadata(
2        userId: String,
3        vehicleId: String,
4        deviceId: String,
5        tag: String,
6        val appId: String,
7        val appVersion: String,
8        val mobileModel: String,
9        val osVersion: String,
10        val isBluetoothOn: Boolean
11    )
```

3. Add speed configs <br/>
To minimise battery power consumption, in SDK you can configure parameters of the `autostart_gps_filters` filter that has the following set of parameters:
- time - periodicity of obtaining coordinates from the GPS system, time in seconds   
- distance - periodicity of obtaining coordinates from the GPS system, distance travelled in metres
- uspeed - upper threshold of vehicle speed, km/h
- dspeed - lower threshold of vehicle speed, km / h
The parameters of the `autostart_gps_filters` filter determine the periodicity with which the mobile application receives and sends GPS system data to the server for further processing. The `autostart_gps_filters` filter starts working  after trip start validation and has the logic:
- if the urrent GPS speed >= dspeed and GPS speed < uspeed and the previous filtering state differs from the current one, then apply a new filter by time = time (in seconds) and filter by distance = distance (in meters). 
- If the previous speed value was within the same limits as the current one, then do not change the filter characteristics.
The autostart_gps_filters filter with default parameter values is a variable with  JSON of the type:

```
1  val configs = """
2    [
3    {
4        "time": 18,
5        "distance": 200,
6        "uspeed": 70,
7        "dspeed": 0
8    },
9    {
10        "time": 18,
11        "distance": 400,
12        "uspeed": 110,
13        "dspeed": 60
14    },
15    {
16        "time": 18,
17        "distance": 600,
18        "uspeed": 150,
19        "dspeed": 100
20    },
21    {
22        "time": 18,
23        "distance": 800,
24        "uspeed": 190,
25        "dspeed": 140
26    },
27    {
28        "time": 18,
29        "distance": 1000,
30        "uspeed": 230,
31        "dspeed": 180
32    },
33    {
34        "time": 18,
35        "distance": 1200,
36        "uspeed": 1000,
37        "dspeed": 220
38    }
39]
40    
41    """
42    DetectorSdk.setSpeedConfig(context, configs)
 
```


4. Working with Bluetooth module (optional) <br/>
SDK provides an opportunity to identify the trip start moment through connecting the smartphone bluetooth module to the vehicle bluetooth. 
<br/>
To work with the smartphone bluetooth module, follow these steps:

```
1    val prefs = context.getSharedPreferences(BTConnectedMonitor.BT_PREFS, Context.MODE_PRIVATE)
2    prefs.edit(commit = true) {
3        putString(BTConnectedMonitor.BT_ADDRESS, address)
4        putString(BTConnectedMonitor.BT_NAME, name)
5    }
```
 
5. SDK initialisation <br/>
To initialise the SDK, follow these steps:

```
1    val config = ConfigBuilder(
2            host = BuildConfig.RECEIVER,
3            port = BuildConfig.RECEIVER_PORT
4        )
5            .setStartValidationTimeout(180)
6            .setStopValidationTimeout(300)
7            .build()
8
9    DetectorSdk.start(context, config, getMetadata())
 ```
 

Next step - [obtaining data about the trips][SAA]




 
 
 [git]: <https://github.com/>
 [TSS]: <mailto:info@kasko2go.com>
 [UAPIK]: <https://developers.google.com/maps/documentation/android-sdk/get-api-key>
 [MSDK]: <https://developers.google.com/maps/documentation/android-sdk/start>
 [FB]: <https://firebase.google.com/docs/android/setup#add-config-file>
 [SAA]: <>
