# node-nmea

[![npm version](https://img.shields.io/npm/v/@drivetech/node-nmea.svg)](https://www.npmjs.com/package/@drivetech/node-nmea)
[![npm downloads](https://img.shields.io/npm/dm/@drivetech/node-nmea.svg)](https://www.npmjs.com/package/@drivetech/node-nmea)
[![Build Status](https://travis-ci.org/Drivetech/node-nmea.svg?branch=master)](https://travis-ci.org/Drivetech/node-nmea)
[![Coverage Status](https://coveralls.io/repos/github/Drivetech/node-nmea/badge.svg?branch=master)](https://coveralls.io/github/Drivetech/node-nmea?branch=master)
[![Maintainability](https://api.codeclimate.com/v1/badges/0dd7acaf25e6b4a6d2e3/maintainability)](https://codeclimate.com/github/Drivetech/node-nmea/maintainability)
[![dependencies Status](https://david-dm.org/drivetech/node-nmea/status.svg)](https://david-dm.org/drivetech/node-nmea)
[![devDependencies Status](https://david-dm.org/drivetech/node-nmea/dev-status.svg)](https://david-dm.org/drivetech/node-nmea?type=dev)

> Parser for NMEA sentences.

Available sentences:
* GPRMC - recommended minimum data for gps
* GPGGA - time, position, and fix related data
* [GPGSA](http://aprs.gids.nl/nmea/#gsa) - GPS DOP and active satellites
* [GPVTG](http://aprs.gids.nl/nmea/#vtg) - Track Made Good and Ground Speed

Example: `$GPRMC,161006.425,A,7855.6020,S,13843.8900,E,154.89,84.62,110715,173.1,W,A*30`

Where:

Value         | Definition
--------------| ----------
161006.425    | Time in UTC 16:10:06.425
A             | Status A=active or V=Void.
7855.6020,S   | Latitude 78°55.6020' N
13843.8900,E  | Longitude 138°43.8900' E
154.89        | Speed over the ground in knots
84.62         | Track angle in degrees True
110715        | Date - 11 of July 2015
173.1,W       | Magnetic Variation in degrees (- West Declination, + East Declination)
A             | FAA Mode A=autonomous, D=differential, E=estimated (dead-reckoning), M=manual input, S=simulated, N=data not valid, P=precise (4.00 and later)
\*30          | The checksum data, always begins with \*

Example: `$GPGGA,120558.916,5058.7457,N,00647.0514,E,2,06,1.7,109.0,M,47.6,M,1.5,0000*71`

Where:

Value         | Definition
--------------| ----------
120558.916    | Time in UTC 12:05:58.916 
5058.7457,N   | Latitude 50°58.7457' N
00647.0514,E  | Longitude 6°47.0514' E
2             | Gps quality. 0=Invalid, 1=GPS fix (SPS), 2=DGPS fix, 3=PPS fix, 4=Real Time Kinematic, 5=Float RTK, 6=estimated (dead reckoning) (2.3 feature), 7=Manual input mode, 8=Simulation mode
06            | Number of satellites
1.7           | HDOP
109.0,M       | Altitude in meters
47.6,M        | Geoidal Separation in meters
1.5           | Age Gps data in seconds
0000          | Reference Station Id
\*71          | The checksum data, always begins with \*


## Installation

```bash
$ npm install @drivetech/node-nmea
```

## Use

[Try on RunKit](https://npm.runkit.com/@drivetech/node-nmea)
```js
const nmea = require('@drivetech/node-nmea')

const raw = '$GPRMC,161006.425,A,7855.6020,S,13843.8900,E,154.89,84.62,110715,173.1,W,A*30'
const data = nmea.parse(raw)
data.valid // true
data.raw // '$GPRMC,161006.425,A,7855.6020,S,13843.8900,E,154.89,84.62,110715,173.1,W,A*30'
data.type // 'RMC'
data.gps // true
data.datetime // Sat Jul 11 2015 13:10:06 GMT-0300 (CLT)
data.loc // { geojson: { type: 'Point', coordinates: [ 138.7315, -78.9267 ] }, dmm: { latitude: '7855.6020,S', longitude: '13843.8900,E' } }
data.speed // { knots: 154.89, kmh: 286.85627999999997 }
data.track // '84.62'
data.magneticVariation // '173.1,W'
data.mode // 'Autonomous'
```

## License

[MIT](https://tldrlegal.com/license/mit-license)
