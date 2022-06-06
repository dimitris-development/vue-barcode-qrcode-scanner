<template>
  <div class="scanner-container">
    <div v-show="!isLoading">
      <video poster="data:image/gif,AAAA" ref="scanner"></video>
      <div class="overlay-element"></div>
      <div class="laser"></div>
    </div>
  </div>
</template>

<script>
import { BrowserMultiFormatReader } from "@zxing/browser";
export default {
  name: "CameraCodeScanner",
  data() {
    return {
      isLoading: true,
      codeScanner: new BrowserMultiFormatReader(),
      isMediaStreamAPISupported:
        navigator &&
        navigator.mediaDevices &&
        "enumerateDevices" in navigator.mediaDevices,
      controls: "",
    };
  },
  emits: ["load", "scan"],
  mounted() {
    if (!this.isMediaStreamAPISupported) {
      throw new Exception("Media Stream API is not supported");
    }

    this.start();
  },
  beforeDestroy() {
    this.controls.stop();
  },
  methods: {
    async start() {
      await this.codeScanner.decodeFromVideoDevice(
        undefined,
        this.$refs.scanner,
        (result, error, controls) => {
          if (this.isLoading) {
            this.controls = controls;
            this.isLoading = false;

            this.$emit("load", {
              controls: this.controls,
              error: error,
              scannerElement: this.$refs.scanner,
              scannerMultiFormatReader: this.codeScanner,
            });
          }

          if (!result) return;
          this.$emit("scan", {
            result: result,
          });
        }
      );
    },
  },
};
</script>

<style scoped>
video {
  max-width: 100%;
  max-height: 100%;
}
.scanner-container {
  position: relative;
}
.overlay-element {
  position: absolute;
  top: 0;
  width: 100%;
  height: 99%;
  background: rgba(30, 30, 30, 0.5);
  -webkit-clip-path: polygon(
    0% 0%,
    0% 100%,
    20% 100%,
    20% 20%,
    80% 20%,
    80% 80%,
    20% 80%,
    20% 100%,
    100% 100%,
    100% 0%
  );
  clip-path: polygon(
    0% 0%,
    0% 100%,
    20% 100%,
    20% 20%,
    80% 20%,
    80% 80%,
    20% 80%,
    20% 100%,
    100% 100%,
    100% 0%
  );
}
.laser {
  width: 60%;
  margin-left: 20%;
  background-color: tomato;
  height: 1px;
  position: absolute;
  top: 40%;
  z-index: 2;
  box-shadow: 0 0 4px red;
  -webkit-animation: scanning 2s infinite;
  animation: scanning 2s infinite;
}
@-webkit-keyframes scanning {
  50% {
    -webkit-transform: translateY(75px);
    transform: translateY(75px);
  }
}
@keyframes scanning {
  50% {
    -webkit-transform: translateY(75px);
    transform: translateY(75px);
  }
}
</style>
