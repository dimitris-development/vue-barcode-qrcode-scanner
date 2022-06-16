# Vue 2 Barcode and QR code scanner

This is a Vue 2 barcode/QR code scanner based on https://github.com/olefirenko/vue-barcode-reader.

In [olefirenko's](https://github.com/olefirenko/) repo there are a few issues that haven't been addressed nor fixed for over a year now, so I decided to rewrite StreamBarcodeReader and fix a few issues I found.
This was meant to be a fork, however due to the inactivity of that repo,
I decided to create a seperate library.

## Installation

The easiest way to use Vue 2 Barcode and QR code scanner is to install it from **npm** or **yarn**.

```sh
npm install vue-barcode-qrcode-scanner --save
```

Or

```sh
yarn add vue-barcode-qrcode-scanner
```

## Usage

The Vue Barcode and QR code scanner works out of the box by just including it.

### Scanning from Video Camera

Once a stream from the users camera is loaded, it is displayed and continuously scanned for barcodes. Results are indicated by the scan event.

```js
import { CameraCodeScanner } from "vue-barcode-qrcode-scanner";
```

In your template you can use this syntax:

```html
<CameraCodeScanner @scan="onScan" @load="onLoad"></CameraCodeScanner>
```

When the Video Camera loads, it emits a load event that exposes all the available
options inside the ZXing BrowserMultiFormatReader API.

```js
methods: {
  onLoad({
    controls,
    scannerElement,
    browserMultiFormatReader
  }) {
    console.log(controls)
    // ---- BrowserMultiFormatReader Controls API ----
    // {
    //   stop: f() // Stops the video stream (Basically turns off the camera)
    // }

    console.log(scannerElement)
    // ---- The ref to the video native element that streams the video-camera output ----
    // <video data-v-73df36b4="" poster="data:image/gif,AAAA" autoplay="true" muted="true" // playsinline="true"></video>

    console.log(browserMultiFormatReader)
    // ---- A reference to the BrowserMultiFormatReader object. ----
    // hints: Map(0)
    // options: {
    //  delayBetweenScanAttempts: 500
    //  delayBetweenScanSuccess: 500
    //  tryPlayVideoTimeout: 5000
    // }
    // reader: MultiFormatReader

    // Please refer to the [ZXing (Zebra crossing) browser documentation](https://github.com/zxing-js/browser)

  },
  onScan({ result, raw }) {
    console.log(result)
    // ---- Scan result ----
    // "http://en.m.wikipedia.org"

    console.log(raw)
    // ---- Raw BrowserMultiFormatReader.decodeFromVideoDevice result ----
    // format: 11
    // numBits: 272
    // rawBytes: Uint8Array(34) [65, 150, 135, 71, 71, 3, 162, 242, 246, 86, 226, 230, 210, 231, 118, 150, 182, 151, 6, 86, 70, 150, 18, 230, 247, 38, 112, 236, 17, 236, 17, 236, 17, 236, buffer: ArrayBuffer(34), byteLength: 34, byteOffset: 0, length: 34, Symbol(Symbol.toStringTag): 'Uint8Array']
    // resultMetadata: Map(2) {2 => Array(1), 3 => 'Q'}
    // resultPoints: (4) [FinderPattern, FinderPattern, FinderPattern, AlignmentPattern]
    // text: "http://en.m.wikipedia.org"
    // timestamp: 1654535879486
  }
}
```

## Fixes

1. According to [Issue 33 in ZXing Browser](https://github.com/zxing-js/browser/issues/33#issuecomment-771716556) I are supposed to use ZXing Browser's BrowserMultiFormatReader in the Browser Layer and not ZXing-JS Library.
   ZXing Browser provides an intuitive API to
   scan anything and is currently being maintained and actively improved upon.
   However using ZXing Library for the Browser Layer is deprecated.
   This change also fixes the "Trying to play video that is already playing." warning.

2. [Issue 19 in vue-barcode-reader](https://github.com/olefirenko/vue-barcode-reader/issues/19) states that even though the video component has been destroyed the camera is still on. This is due to beforeUnmount option not existing prior to Vue 3.2.7.
   I are using beforeDestroy instead for Vue 2 support.

3. vue-barcode-reader isn't minified by default and contributions aren't easy make due to the absence of development scripts, so I compiled the library with [vue-sfc-rollup](https://github.com/team-innovation/vue-sfc-rollup) and [Vue CLI](https://cli.vuejs.org/)

## How to contribute

Feel free to submit issues and enhancement requests, however if you want
to contribute just follow these steps:

1. **Fork** this repo on GitHub
2. **Clone** the project to your own machine
3. Just run:

   ```sh
   npm init
   npm run serve
   ```

   Or

   ```sh
   yarn
   yarn serve
   ```

   and commit whatever changes you want to make.

   At the end run

   ```sh
   npm run build
   ```

   Or

   ```sh
   yarn build
   ```

to build the library and commit it as well

4. Push your work back to your fork

5. Submit a PR so that I can review your changes.
