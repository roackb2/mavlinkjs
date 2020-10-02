# What's This

This repository is an npm module and a JavaScript MAVLink implementation to parse MAVLink messages, generated using [Parrot MAVLink repository](https://github.com/Parrot-Developers/mavlink/tree/master/pymavlink/generator/javascript),
with the multiple dialects, including `all` and `common` for now, using Mavlink v1.0 protocol.

# Import


```JavaScript
const { mavlink10: mavlink, MAVLink10Processor: MAVLink } = require('mavlinkjs/mavlink_all_v1');
```

or

```JavaScript
import { mavlink10 as mavlink, MAVLink10Processor as MAVLink } from 'mavlinkjs/mavlink_all_v1';
```

# Usage

Suppose you're using serial port to read data from your flight control, you could use Mavlink parser as following:

```JavaScript
const SerialPort = require('serialport');
const { mavlink10: mavlink, MAVLink10Processor: MAVLink } = require('mavlinkjs/mavlink_all_v1');

const mavlinkParser = new MAVLink(null, 0, 0);
mavlinkParser.on('message', function(msg) {
    // the parsed message is here
    console.log(msg);
});

mavlinkParser.on('HEARTBEAT', function(msg) {
    // you could listen on specific message type
    console.log(msg.name)
    // outputs 'HEARTBEAT'
});

const serialport = new SerialPort('/dev/tty.usbserial-1430', {
    57600,
    autoOpen: true
})
serialport.on('error', function(err) {
    logger.warn(`Error in serial port: ${err.message}`)
})
// When the connection issues a "got data" event, try and parse it
serialport.on('data', function(data) {
    // pass to mavlinkParser to parse the raw data
    mavlinkParser.parseBuffer(data)
});
```
