# wiegand

Decoder for [wiegand](https://en.wikipedia.org/wiki/Wiegand_interface) readers on GPIO.
Currently works on linux only, but can be tested on other platforms.

## installation

```bash
$ npm install --save wiegand-rfid-reader
#OR
$ npm install --save https://github.com/7aman/wiegand-rfid-reader.git
```

## usage

```js
const wiegand = require('wiegand-rfid-reader');
const reader = wiegand();
const Gpio = require('onoff').Gpio;

//export d0, d1 pins
const d0 = 17, d1 = 18;
const wgnd0 = new Gpio(d0, "in", "both");
const wgnd1 = new Gpio(d1, "in", "both");
reader.begin({ d0: d0, d1: d1});

reader.on('id', id => {
    console.log('Got', id, 'from RFID reader');
});

reader.on('error', err => {
    console.log(err);
});

process.on('SIGINT', () => {
  wgnd0.unexport();
  wgnd1.unexport();
});
```

## api

### class: Wiegand

inherits EventEmitter

#### method: begin([options], [callback])

default options:
```js
{
    // RaspberryPI default
    d0: 17,
    d1: 18,
}
```

`callback` will fire on the `ready` event or if an error happens during the process. Errors will be emited on the `error` event.

#### method: stop([callback])

stop polling for changes to GPIO. Will callback when done and emit a `stop` event.

#### event: 'ready'

#### event: 'data'

#### event: 'error'

#### event: 'id'

#### event: 'stop'

## license

[The MIT License](https://opensource.org/licenses/MIT)
