<script>
import Vue from "vue";
import { CameraCodeScanner } from "@/entry.esm";

// Uncomment import and local "components" registration if library is not registered globally.

export default Vue.extend({
  components: { CameraCodeScanner },
  name: "ServeDev",
  data() {
    return {
      shouldScan: true,
      controls: "",
    };
  },
  methods: {
    onScan({ result }) {
      console.log(result);
      this.shouldScan = false;
    },
    onLoad({
      scanner,
      controls,
      error,
      scannerElement,
      scannerMultiFormatReader,
    }) {
      console.log(
        scanner,
        controls,
        error,
        scannerElement,
        scannerMultiFormatReader
      );
      this.controls = controls;
    },
  },
});
</script>

<template>
  <div id="app">
    <CameraCodeScanner v-if="shouldScan" @load="onLoad" @scan="onScan" />
  </div>
</template>
