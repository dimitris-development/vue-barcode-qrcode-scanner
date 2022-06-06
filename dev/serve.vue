<script>
import Vue from "vue";
import { ImageBarcodeScanner, StreamBarcodeScanner } from "@/entry.esm";

// Uncomment import and local "components" registration if library is not registered globally.

export default Vue.extend({
  components: { ImageBarcodeScanner, StreamBarcodeScanner },
  name: "ServeDev",
});
</script>

<template>
  <div id="app">
    <ImageBarcodeScanner />
    <StreamBarcodeScanner />
  </div>
</template>
