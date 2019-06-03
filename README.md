# IOTA Compress

IOTA Trytes use a very specific dictionary in their construction, to provide a mechanism by which they can be more efficiently stored or transmitted the IOTA compression library was created.

By looking at the domain and the specific way in which tryte data is used we created an algorithm that works extremely efficiently in the most common use cases and. The algorithm is also implemented in such a way as to make it lightweight enough to be used by embedded devices.

For a comparison of this library against other well known algorithms please read [./docs/comparison.md](./docs/comparison.md)

## Installing

Install this package using the following commands:

```shell
npm install @iota/compress
```

or

```shell
yarn add @iota/compress
```

## Example Usage

```js
const iotaCompress = require('@iota/compress');

const iotaCompress = require('../dist/iota-compress');

let trytes = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ9';
console.log('trytes', trytes);
console.log('trytes length', trytes.length);

let compressedBytes = iotaCompress.compress(trytes);
console.log('compressed', compressedBytes);
console.log('compressed length', compressedBytes.length);

let decompressed = iotaCompress.decompress(compressedBytes);
console.log('decompressed', decompressed);
console.log('matches', decompressed === trytes);

trytes = '9'.repeat(2673);
console.log('trytes', trytes);
console.log('trytes length', trytes.length);

compressedBytes = iotaCompress.compress(trytes);
console.log('compressed', compressedBytes);
console.log('compressed length', compressedBytes.length);

decompressed = iotaCompress.decompress(compressedBytes);
console.log('decompressed', decompressed);
console.log('matches', decompressed === trytes);
```

Will output:

```shell
trytes ABCDEFGHIJKLMNOPQRSTUVWXYZ9
trytes length 27
compressed <Buffer 4f 0f fb c9 62 f0 8c c3 9a ed 5a 2a 2d f6 6b 1c 60 00>
compressed length 18
decompressed ABCDEFGHIJKLMNOPQRSTUVWXYZ9
matches true
trytes 9999999...999999999
trytes length 2673
compressed <Buffer 01 3d 09 80 00>
compressed length 5
decompressed 9999999...999999999
matches true
compressed 241 <Buffer 9f a8 3e f9 0d 07 9f 01 ab bc 7e ee 7f dd 02 27 ee e7 fd d0 22 7e ee 7f dd 02 27 ee e7 fd d0 22 7e ee 7f dd 02 27 ee e7 fd d0 22 7e ee 7f dd 02 27 ee ... >
matches true
```

## API Reference

See the API reference for the Javascript implementation [here](./docs/api.md).

## Examples

To see the code in use there are some samples in the examples folder.

* [simple](./examples/simple/) - The example code show above.
* [huffmanTrainer](./examples/huffmanTrainer/) - The code which creates the static huffman tables by analyzing zmq stream.
* [zmq](./examples/zmq/) - Example which monitors zmq stream and shows the potential compression ratios achieved with different algorithms.

## Results

The current algorithm `trytes` compared with some industry standard algorithms using the zmq example.

After processing 10,000 transactions, these are the averages.

```
brotli - Compressed Size: 669 bytes, Saving Size: 75.0 %, Time: 4ms, Max: 1602 bytes, Min: 167 bytes
deflate - Compressed Size: 694 bytes, Saving Size: 74.0 %, Time: 1ms, Max: 1646 bytes, Min: 180 bytes
huffman - Compressed Size: 919 bytes, Saving Size: 65.6 %, Time: 1ms, Max: 1654 bytes, Min: 525 bytes
lz - Compressed Size: 764 bytes, Saving Size: 71.4 %, Time: 1ms, Max: 1631 bytes, Min: 294 bytes
lz4 - Compressed Size: 1089 bytes, Saving Size: 59.3 %, Time: 1ms, Max: 2653 bytes, Min: 241 bytes
bzip2 - Compressed Size: 749 bytes, Saving Size: 72.0 %, Time: 4ms, Max: 1719 bytes, Min: 249 bytes
trytes - Compressed Size: 662 bytes, Saving Size: 75.2 %, Time: 1ms, Max: 1593 bytes, Min: 183 bytes
```

## License

MIT License - Copyright (c) 2019 IOTA Stiftung
