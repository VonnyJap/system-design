#### Requirements
- Functional
- Nonfunctional

#### API
- updateDriverLocation(driver_id, oldlong, oldlat, newlong, newlat)
- findDrivers(rider_id, lat, long)
- requestRide(rider_id, lat, long, dropOffLat, dropOffLong, vehicle)
- showETA(driver_id, eta)
- confirmPickup(driver_id, rider_id, timestamp)
- showTripUpdates(tripID, riderID, driverID, driverlat, driverlong, time_elapsed, time_remaining)
- endTrip(tripID, riderID, driverID ,time_elapsed, lat, long)


#### Quadtree server 
- purpose: for geolocation services

