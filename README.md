# Goal

- Map all of the roadtrips that I've taken with Soledad: my 2002 Volkswagen Passat.

2007 - 2010 Streamwood, IL to Peoria, IL (multiple trips)

May 2009 Streamwood, IL to Washington, DC
July 2009 Washington, DC to Wildwood, NJ
August 2009 Washington DC to Streamwood, IL

May 2010 Streamwood, IL to Washington, DC
July 2010 Washington, DC to Wildwood, NJ
August 2010 Washington DC to Streamwood, IL

??? Streamwood, IL to Charlotte, NC

Feb 2011 Streamwood, IL to Redwood City, CA
May 2011 Redwood City, CA to Austin, TX
May 2013 Austin, TX to Portland, OR

2012 Austin, TX to San Antonio, TX

STL 2007 April 15


1. List of all travels and cities + dates
2. Use Esri Leaflet to create map
https://github.com/Esri/esri-leaflet
  - first problem: spent ~30min manually getting lat/long for all cities.
  Realized this would only put a marker on those lat/longs.
  Need to find a way to get routes. Looking into Google Maps API
  http://stackoverflow.com/questions/21329396/how-to-display-google-api-directions-polylines-with-leaflet
3. Google Maps API 4:36pm
https://developers.google.com/maps/
Google Directions API https://developers.google.com/maps/documentation/directions/
API Key: https://code.google.com/apis/console/?noredirect&pli=1#project:898595756886:access
  Need directions data, will pull from Google Maps API.
  From there, save it and add data to esri-leaflet map, apparently using polyline.
  - Will need to use waypoints in GMaps API
- Progress? Got an API key, just don't know how to run requests from terminal. Need to figure this out

Day 2: TIME 05/12/2014
- Nate showed me curl
curl "https://maps.googleapis.com/maps/api/directions/json?origin=Chicago,IL&destination=Peoria,IL&sensor=false&key=AIzaSyANL6ut9sYrn5t9-gVm2kDcoqwBr_FVavc"
and this returns encoded data.
- Will now try to use jieter's Leaflet.encoded to decode the polyline
https://github.com/jieter/Leaflet.encoded

- First step is to get Esri Leaflet example up and running. I ran into a snag because I did not save before running python -m SimpleHTTPServer. Also, I was in the wrong folder so it was running inside of a directory and not the whole project.

- All up and good. Now to spin up the polyline decoder example.
- 5:34pm = Success!!

- Will grab encoded polyline from google and try to add them to the decoded example.
Results: Success - [Screenshots]

- !todo:
1. Get encoded roadtrip information from Google Directions API from 'RoadtipLatLong.xlsx' worksheet
2. Figure out how to add Polyline decoder to Esri-Leaflet
3. Add unencoded trips to Esri-Leaflet from large file
4. Style so that each trip is a layer (looks different/can be differentiated)

Trip: Streamwood, IL to Hastings, NE
curl "https://maps.googleapis.com/maps/api/directions/json?origin=42.032942,-88.194049&destination=40.545896,-98.420731&sensor=false&key=AIzaSyANL6ut9sYrn5t9-gVm2kDcoqwBr_FVavc"

- by adding all blocks of "points" from GDirections API in var encoded = "" in decoder, I can create the route. [Screenshots]
- Actually, by just including the last "points", it recreates the whole path. Even easier!

Trip: Hastings, NE to Fort Bridger, WY
curl "https://maps.googleapis.com/maps/api/directions/json?origin=40.545896,-98.420731&destination=41.317013,-110.383866&sensor=false&key=AIzaSyANL6ut9sYrn5t9-gVm2kDcoqwBr_FVavc"

Trip: Fort Bridger, WY to Winnemuca, NV
curl "https://maps.googleapis.com/maps/api/directions/json?origin=41.317013,-110.383866&destination=40.97264,-117.735527&sensor=false&key=AIzaSyANL6ut9sYrn5t9-gVm2kDcoqwBr_FVavc"

Trip: Winnemuca, NV to Redwood City, CA [Screenshots]
curl "https://maps.googleapis.com/maps/api/directions/json?origin=40.97264,-117.735527&destination=37.452785,-122.225159&sensor=false&key=AIzaSyANL6ut9sYrn5t9-gVm2kDcoqwBr_FVavc"

-- Note: When addind this last line ^^ it only shows the Winnemuca, NV to Redlands, CA portion of the trip. Don't know why. Maybe the decoder has a limit of how many legs it can show/handle? Will test later.

- !todo
1. Figure out how to use waypoints.

TIME 6:27pm 05/12/2014
- Waypoints
Trip: Streamwood, IL to Fort Bridger, WY with waypoint Hastings, NE
curl "https://maps.googleapis.com/maps/api/directions/json?origin=42.032942,-88.194049&destination=41.317013,-110.383866&waypoints=40.545896,-98.420731&sensor=false&key=AIzaSyANL6ut9sYrn5t9-gVm2kDcoqwBr_FVavc"

TIME 6:32pm 05/12/2014
- Waypoint Success!!!
=== End ===

TIME 6:58pm 05/12/2014
Trip: Streamwood, IL to Redwood City, CA with waypoints Hastings, NE, Fort Bridger, WY, and Winnemuca, NV
curl "https://maps.googleapis.com/maps/api/directions/json?origin=42.032942,-88.194049&destination=37.452785,-122.225159&waypoints=40.545896,-98.420731|41.317013,-110.383866|40.97264,-117.735527&sensor=false&key=AIzaSyANL6ut9sYrn5t9-gVm2kDcoqwBr_FVavc"
- Waypoints Success!!! [Screenshot]

Day 3 TIME 10:19am 05/13/2014
- How do you write a script to do all of this programatically?
1. Take the latitude and longitude from a CSV
2. Put into Google Directions curl request format
3. Take "points" from "overview_polyline" and decode using polyline.encoded.js
4. Display on Esri map

TIME 11:06am 05/13/2014
- decoded polyline was successfully put onto esri leaflet!

Trip: Redwood City, CA to Austin, TX with waypoints Los Angeles, CA, Gallup, NM, and Lubbock, TX
curl "https://maps.googleapis.com/maps/api/directions/json?origin=37.45323,-122.225205&destination=30.259438,-97.75615&waypoints=34.094392,-118.211009|35.532331,-108.759759|33.592029,-101.939762&sensor=false&key=AIzaSyANL6ut9sYrn5t9-gVm2kDcoqwBr_FVavc"

Trip Streamwood, IL to Wildwood, NJ with waypoint Washington, D.C.
curl "https://maps.googleapis.com/maps/api/directions/json?origin=42.032942,-88.194049&destination=38.991369,-74.814674&waypoints=38.930697,-77.02243&sensor=false&key=AIzaSyANL6ut9sYrn5t9-gVm2kDcoqwBr_FVavc"

Trip Streamwood, IL to Charlotte, NC
curl "https://maps.googleapis.com/maps/api/directions/json?origin=42.032942,-88.194049&destination=35.116489,-80.722373&sensor=false&key=AIzaSyANL6ut9sYrn5t9-gVm2kDcoqwBr_FVavc"

Trip Austin, TX to Portland, OR
curl "https://maps.googleapis.com/maps/api/directions/json?origin=30.28138,-97.760474&destination=45.521029,-122.651621&waypoints=30.201107,-103.250379|29.274245,-103.293008|35.083885,-106.663612|36.049966,-112.114302|38.610662,-109.557995&sensor=false&key=AIzaSyANL6ut9sYrn5t9-gVm2kDcoqwBr_FVavc"


Adding gh-pages









