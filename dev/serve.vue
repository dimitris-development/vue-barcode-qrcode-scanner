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
      camera: "front",
    };
  },
  mounted() {
    setInterval(() => {
      if (this.camera === "front") {
        this.camera = "back";
      } else {
        this.camera = "front";
      }
    }, 5000);
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
    <CameraCodeScanner
      v-if="shouldScan"
      :camera="camera"
      @load="onLoad"
      @scan="onScan"
    />
  </div>
</template>
