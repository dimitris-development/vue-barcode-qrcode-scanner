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
    onScan({ result, raw }) {
      console.log(result, raw);
      this.shouldScan = false;
    },
    onLoad({ controls, scannerElement, browserMultiFormatReader }) {
      console.log(controls, scannerElement, browserMultiFormatReader);
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
