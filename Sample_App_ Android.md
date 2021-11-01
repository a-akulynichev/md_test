## _Sample App. Android_

The Open source solution SDK for Android allows a user to get the following information:

## 1. Data on trips made over a specified period of time.
Use the following API method  to receive information on trips made over a specified period of time:  
```
1  ScoringSdk.getTrips(
2            vehicleUUID = getVehicleId(),
3            deviceId = getDeviceId(),
4            userId = getUserId(),
5            to = DateTime.now().millis / 1000,
6            from = null
7    )
8
9    fun getTrips(
10        vehicleUUID: UUID, // vehicle id for identification
11        deviceId: UUID, // device id for identification
12        userId: UUID, // user id for identification
13        to: Long?, // date to filed for filtering
14        from: Long?, // date from filed for filtering
15        limit: Int = 20, // limit of records in response for pagination
16        offset: Int = 0 // offset of records for pagination
17    ): Single<ScoringTrips>
18
19    data class ScoringTrips(
20        val limit: Long?, // limit of records in response for pagination
21        val totalCount: Long, // total count of records in response for pagination
22        val values: List<ScoringTrip>
23    )
24
25    data class ScoringTrip(
26        val tripData: TripData,
27        val scoring: ScoreData?
28    )
29
30    data class TripData(
31        val id: UUID, // Unique identifier of the trip
32        val userId: String?, // Unique identifier of the user
33        val deviceId: String?, // Unique identifier of the device
34        val vehicleId: String?, // Unique identifier of the vehicle
35        val startTimestamp: DateTime, // Start time of the trip (UNIX timestamp)
36        val finishTimestamp: DateTime, // Finish time of the trip (UNIX timestamp)
37        val tag: TripDataTag // A value that the trip has been tagged with
38        val isBluetoothOn: true, // Is bluetooth on
39        val isBluetoothConnectionEstablished: true // Is bluetooth connection establihed
40    )

```

## 2. Detailed information on the trip that has been made.
Use the following API method to receive information on a specified trip:
```
1  ScoringSdk.getTripsInfo(tripId)
2
3    fun getTripsInfo(
4        tripId: UUID, // id of trip
5        loadAllEvents: Boolean = true, // flag for load all events
6        mapToRoads: Boolean = true // flag for mapping
7    ): Single<ScoringTripDetails>
8
9    data class ScoringTripDetails(
10        val trip: ScoredTrip, // trip details
11        val events: List<TelemetryEvent>? = null, // trip events
12        val penalties: List<Penalty>, // trip penalties
13        val startAddress: String, // start trip address
14        val endAddress: String, // end trip address
15        val interactiveMapEnabled: Boolean? // ?
16    )
17
18    data class TelemetryEvent(
19        val timestamp: Long, // Date and time of the event (UNIX timestamp)
20        val latitude: Double, // Latitude of the location point (in signed degrees format)
21        val longitude: Double, // Longitude of the location point (in signed degrees format)
22        val speedKph: Double, // The speed of the object at the specified time (in kilometers per hour)
23        val heading: Double, // ompass direction in which the object's bow or nose is pointed (0 or 360 indicates a direction toward true North)
24        val accuracy: Double // The accuracy of the location information
25    )
26
27    data class Penalty(
28        val timestamp: Long, // Date and time of the event (UNIX timestamp)
29        val type: PenaltyType, // The type of the event
30        val durationMs: Long, // The duration (in milliseconds) of the event
31        val value: Double // Indicates the severity of the event (depends on the type)
32    )
33
34    enum class PenaltyType {
35        BRAKING,
36        ACCELERATION,
37        OVER_SPEED,
38    }
```


## 3. The weighted average of the score and the total value of vehicle mileage over a certain period of time.
Use the following API method to receive information:   

```
1  ScoringSdk.getScoring(
2            vehicleUUID = getVehicleId(),
3            deviceId = getDeviceId(),
4            userId = getUserId(),
5            dateFrom = 0
6    )
7
8    fun getScoring(
9        vehicleUUID: UUID, // vehicle id for identification
10        deviceId: UUID, // device id for identification
11        userId: UUID, // user id for identification
12        dateFrom: Long,  // date from filed for filtering
13        tag: TripDataTag = TripDataTag.DRIVER_IN_MY_CAR // tag field for filtering
14    ): Single<ScoreData>
15
16    data class ScoreData(
17        val scorePercent: Double, // score percent for all selected trips
18        val durationSec: Long, // duration in sec for all selected trips
19        val distanceMeters: Long, // distance in meters for all selected trips
20        val tripsCount: Long // count of for all selected trips
21    )
```

## 4. Building a vehicle route/routes, getting information about dangerous road sections.
Use the following API method to get the information about vehicle routes and risks related to traveling along each of those routes:<br/>
Request method:  

```
1  RoutesSdk.getRoutes(
2        originLat: Double,
3        originLng: Double,
4        destinationLat: Double,
5        destinationLng: Double,
6        departureTime: Long?,
7  )
```

Response parameters:
```
1  @Parcelize
2  data class Route(
3    val distance: Double,
4    val duration: Long,
5    val accidentRisk: Double,
6    val riskCountForUi: Int,
7    val riskCountForUiColor: String,
8    val inactiveRouteColor: String,
9    val lowRiskDistance: Double,
10    val lowRiskPercentage: Double,
11    val normalRiskDistance: Double,
12    val normalRiskPercentage: Double,
13    val highRiskDistance: Double,
14    val highRiskPercentage: Double,
15    val highRiskLinks: List<HighRiskLink>,
16    val road: Road,
17    val waypoints: List<LatLng>
18  ) : Parcelable
19
20  @Parcelize
21  data class HighRiskLink(
22    val description: String,
23    val accidentsCount: Int,
24    val accidentsYears: String,
25) : Parcelable
26
27  @Parcelize
28  data class Road(
29    val baseRoute: List<LatLng>,
30    val baseColor: String,
31    val lowRoute: List<List<LatLng>>,
32    val lowColor: String,
33    val normalRoute: List<List<LatLng>>,
34    val normalColor: String,
35    val highRoute: List<List<LatLng>>,
36    val highColor: String,
37  ) : Parcelable
38
39  @Parcelize
40  data class RoutesWrapper(
41    val routes: List<Route>
42) : Parcelable
```


