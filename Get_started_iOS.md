## _Sample App. Android_

The Open source solution SDK for Android allows a user to get the following information:

## 1. Data on trips made over a specified period of time.
Use the following API method  to receive information on trips made over a specified period of time:  
```
 ScoringSdk.getTrips(
           vehicleUUID = getVehicleId(),
           deviceId = getDeviceId(),
           userId = getUserId(),
           to = DateTime.now().millis / 1000,
           from = null
   )

   fun getTrips(
        vehicleUUID: UUID, // vehicle id for identification
        deviceId: UUID, // device id for identification
        userId: UUID, // user id for identification
        to: Long?, // date to filed for filtering
        from: Long?, // date from filed for filtering
        limit: Int = 20, // limit of records in response for pagination
        offset: Int = 0 // offset of records for pagination
    ): Single<ScoringTrips>

    data class ScoringTrips(
        val limit: Long?, // limit of records in response for pagination
        val totalCount: Long, // total count of records in response for pagination
        val values: List<ScoringTrip>
    )

    data class ScoringTrip(
        val tripData: TripData,
        val scoring: ScoreData?
    )

    data class TripData(
        val id: UUID, // Unique identifier of the trip
        val userId: String?, // Unique identifier of the user
        val deviceId: String?, // Unique identifier of the device
        val vehicleId: String?, // Unique identifier of the vehicle
        val startTimestamp: DateTime, // Start time of the trip (UNIX timestamp)
        val finishTimestamp: DateTime, // Finish time of the trip (UNIX timestamp)
        val tag: TripDataTag // A value that the trip has been tagged with
        val isBluetoothOn: true, // Is bluetooth on
        val isBluetoothConnectionEstablished: true // Is bluetooth connection establihed
    )

```

## 2. Detailed information on the trip that has been made.
Use the following API method to receive information on a specified trip:
```
 ScoringSdk.getTripsInfo(tripId)

   fun getTripsInfo(
       tripId: UUID, // id of trip
       loadAllEvents: Boolean = true, // flag for load all events
       mapToRoads: Boolean = true // flag for mapping
   ): Single<ScoringTripDetails>

   data class ScoringTripDetails(
        val trip: ScoredTrip, // trip details
        val events: List<TelemetryEvent>? = null, // trip events
        val penalties: List<Penalty>, // trip penalties
        val startAddress: String, // start trip address
        val endAddress: String, // end trip address
        val interactiveMapEnabled: Boolean? // ?
    )

    data class TelemetryEvent(
        val timestamp: Long, // Date and time of the event (UNIX timestamp)
        val latitude: Double, // Latitude of the location point (in signed degrees format)
        val longitude: Double, // Longitude of the location point (in signed degrees format)
        val speedKph: Double, // The speed of the object at the specified time (in kilometers per hour)
        val heading: Double, // ompass direction in which the object's bow or nose is pointed (0 or 360 indicates a direction toward true North)
        val accuracy: Double // The accuracy of the location information
    )

    data class Penalty(
        val timestamp: Long, // Date and time of the event (UNIX timestamp)
        val type: PenaltyType, // The type of the event
        val durationMs: Long, // The duration (in milliseconds) of the event
        val value: Double // Indicates the severity of the event (depends on the type)
    )

    enum class PenaltyType {
        BRAKING,
        ACCELERATION,
        OVER_SPEED,
    }
```


## 3. The weighted average of the score and the total value of vehicle mileage over a certain period of time.
Use the following API method to receive information:   

```
 ScoringSdk.getScoring(
           vehicleUUID = getVehicleId(),
           deviceId = getDeviceId(),
           userId = getUserId(),
           dateFrom = 0
   )

   fun getScoring(
       vehicleUUID: UUID, // vehicle id for identification
        deviceId: UUID, // device id for identification
        userId: UUID, // user id for identification
        dateFrom: Long,  // date from filed for filtering
        tag: TripDataTag = TripDataTag.DRIVER_IN_MY_CAR // tag field for filtering
    ): Single<ScoreData>

    data class ScoreData(
        val scorePercent: Double, // score percent for all selected trips
        val durationSec: Long, // duration in sec for all selected trips
        val distanceMeters: Long, // distance in meters for all selected trips
        val tripsCount: Long // count of for all selected trips
    )
```

## 4. Building a vehicle route/routes, getting information about dangerous road sections.
Use the following API method to get the information about vehicle routes and risks related to traveling along each of those routes:<br/>
Request method:  

```
 RoutesSdk.getRoutes(
       originLat: Double,
       originLng: Double,
       destinationLat: Double,
       destinationLng: Double,
       departureTime: Long?,
 )
```

Response parameters:
```
 @Parcelize
 data class Route(
   val distance: Double,
   val duration: Long,
   val accidentRisk: Double,
   val riskCountForUi: Int,
   val riskCountForUiColor: String,
   val inactiveRouteColor: String,
   val lowRiskDistance: Double,
   val lowRiskPercentage: Double,
   val normalRiskDistance: Double,
   val normalRiskPercentage: Double,
   val highRiskDistance: Double,
   val highRiskPercentage: Double,
   val highRiskLinks: List<HighRiskLink>,
   val road: Road,
   val waypoints: List<LatLng>
 ) : Parcelable

 @Parcelize
 data class HighRiskLink(
   val description: String,
   val accidentsCount: Int,
   val accidentsYears: String,
 ) : Parcelable

 @Parcelize
 data class Road(
   val baseRoute: List<LatLng>,
   val baseColor: String,
   val lowRoute: List<List<LatLng>>,
   val lowColor: String,
   val normalRoute: List<List<LatLng>>,
   val normalColor: String,
   val highRoute: List<List<LatLng>>,
   val highColor: String,
 ) : Parcelable

 @Parcelize
 data class RoutesWrapper(
   val routes: List<Route>
) : Parcelable
```


