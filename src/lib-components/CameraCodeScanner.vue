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
  props: {
    camera: {
      type: String,
      default: "",
    },
  },
  data() {
    return {
      isLoading: true,
      codeScanner: new BrowserMultiFormatReader(),
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
  computed: {
    isMediaStreamAPISupported() {
      return (
        navigator &&
        navigator.mediaDevices &&
        "enumerateDevices" in navigator.mediaDevices
      );
    },
    constraints() {
      if (this.camera === "front") return { video: { facingMode: "user" } };
      else if (this.camera === "back")
        return { video: { facingMode: "enviroment" } };
      return "";
      console.log(this.constraints);
    },
  },
  methods: {
    start() {
      if (this.constraints) {
        console.log(this.constraints);
        this.codeScanner.decodeFromConstraints(
          this.constraints,
          this.$refs.scanner,
          this.callback
        );
      } else {
        this.codeScanner.decodeFromVideoDevice(
          undefined,
          this.$refs.scanner,
          this.callback
        );
      }
    },
    callback(result, error, controls) {
      if (this.isLoading) {
        this.controls = controls;
        this.isLoading = false;

        this.$emit("load", {
          controls: this.controls,
          scannerElement: this.$refs.scanner,
          browserMultiFormatReader: this.codeScanner,
        });
      }

      if (!result) return;
      this.$emit("scan", {
        result: result.text,
        raw: result,
      });
    },
  },
  watch: {
    camera(newVal, oldVal) {
      if (newVal !== oldVal) {
        this.isLoading = true;
        this.controls.stop();
        this.start();
      }
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
