<script>
import Vue from "vue";
import ImageBarcodeScanner from "../src/lib-components/ImageBarcodeScanner.vue";
import StreamBarcodeScanner from "../src/lib-components/StreamBarcodeScanner.vue";
// Uncomment import and local "components" registration if library is not registered globally.
// import { ImageBarcodeScanner } from "@/entry.esm";

export default Vue.extend({
  components: { ImageBarcodeScanner, StreamBarcodeScanner },
  name: "ServeDev",
  // components: {
  //   ImageBarcodeScanner,
  // },
});
</script>

<template>
  <div id="app">
    <ImageBarcodeScanner />
    <StreamBarcodeScanner />
  </div>
</template>
