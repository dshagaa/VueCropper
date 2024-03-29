<template>
	<img :src="src" alt="" ref="cropper" :height="height">
</template>

<script>
import Cropper from "cropperjs";
require("cropperjs/dist/cropper.min.css");
export default {
  name: "vue-cropper",
  props: {
    src: {
      type: String,
      default: ""
    },
    height: {
      type: String,
      default: "200"
    },
    viewMode: {
      type: Number,
      default: 0
    },
    dragMode: {
      type: String,
      default: "crop"
    },
    aspectRatio: {
      type: Number,
      default: NaN
    },
    data: {
      type: Object,
      default: null
    },
    preview:{
      type: [String,Array,Object,Element],
      default: ''
    },
    responsive: {
      type: Boolean,
      default: true
    },
    restore: {
      type: Boolean,
      default: true
    },
    checkCrossOrigin: {
      type: Boolean,
      default: true
    },
    checkOrientation: {
      type: Boolean,
      default: true
    },
    modal: {
      type: Boolean,
      default: true
    },
    guides: {
      type: Boolean,
      default: true
    },
    center: {
      type: Boolean,
      default: true
    },
    highlight: {
      type: Boolean,
      default: true
    },
    background: {
      type: Boolean,
      default: true
    },
    autoCrop: {
      type: Boolean,
      default: true
    },
    autoCropArea: {
      type: Number,
      default: 0.8
    },
    movable: {
      type: Boolean,
      default: true
    },
    rotatable: {
      type: Boolean,
      default: true
    },
    scalable: {
      type: Boolean,
      default: true
    },
    zoomable: {
      type: Boolean,
      default: true
    },
    zoomOnTouch: {
      type: Boolean,
      default: true
    },
    zoomOnWheel: {
      type: Boolean,
      default: true
    },
    wheelZoomRatio: {
      type: Number,
      default: 0.1
    },
    cropBoxMovable: {
      type: Boolean,
      default: true
    },
    cropBoxResizable: {
      type: Boolean,
      default: true
    },
    toggleDragModeOnDblclick: {
      type: Boolean,
      default: true
    },
    minContainerWidth: {
      type: Number,
      default: 200
    },
    minContainerHeight: {
      type: Number,
      default: 100
    },
    minCanvasWidth: {
      type: Number,
      default: 0
    },
    minCanvasHeight: {
      type: Number,
      default: 0
    },
    minCropBoxWidth: {
      type: Number,
      default: 0
    },
    minCropBoxHeight: {
      type: Number,
      default: 0
    },
    ready: {
      type: Function,
      default: null
    },
    cropstart: {
      type: Function,
      default: null
    },
    cropmove: {
      type: Function,
      default: null
    },
    cropend: {
      type: Function,
      default: null
    },
    crop: {
      type: Function,
      default: null
    },
    zoom: {
      type: Function,
      default: null
    }
  },
  mounted() {
    this.init();
  },
  methods: {
    init(options = {}) {
      let opt = (Object.keys(options).length === 0) ? this.options() : options;
      new Cropper(this.$refs.cropper, opt);
    },
    options() {
      const opt = {};
      for (const [key, value] of Object.entries(this.$props)) {
          opt[key] = value;
      }
      return opt;
    },
    cropFn() {
      this.$refs.cropper.cropper.crop();
    },
    reset() {
      this.$refs.cropper.cropper.reset();
    },
    clear() {
      this.$refs.cropper.cropper.clear();
    },
    replace(url, hasSameSize = false) {
      this.$refs.cropper.cropper.replace(url, hasSameSize);
    },
    enable() {
      this.$refs.cropper.cropper.enable();
    },
    disable() {
      this.$refs.cropper.cropper.disable();
    },
    destroy() {
      this.$refs.cropper.cropper.destroy();
    },
    move(offsetX, offsetY = null) {
      if (offsetY === null) {
        offsetY = offsetX;
      }
      this.$refs.cropper.cropper.move(offsetX, offsetY);
    },
    moveTo(x, y = null) {
      if (y === null) {
        y = x;
      }
      this.$refs.cropper.cropper.moveTo(x, y);
    },
    zoomFn(ratio) {
      this.$refs.cropper.cropper.zoom(ratio);
    },
    zoomTo(ratio, pivot = {}) {
      this.$refs.cropper.cropper.zoomTo(ratio, pivot);
    },
    rotate(degree) {
      this.$refs.cropper.cropper.rotate(degree);
    },
    rotateTo(degree) {
      this.$refs.cropper.cropper.rotateTo(degree);
    },
    scale(scaleX, scaleY = null) {
      if (scaleY === null) {
        scaleY = scaleX;
      }
      this.$refs.cropper.cropper.scale(scaleX, scaleY);
    },
    scaleX(x) {
      this.$refs.cropper.cropper.scaleX(x);
    },
    scaleY(y) {
      this.$refs.cropper.cropper.scaleY(y);
    },
    getData(rounded = false) {
      return this.$refs.cropper.cropper.getData(rounded);
    },
    setData(data) {
      this.$refs.cropper.cropper.setData(data);
    },
    getContainerData() {
      return this.$refs.cropper.cropper.getContainerData();
    },
    getImageData() {
      return this.$refs.cropper.cropper.getImageData();
    },
    getCanvasData() {
      return this.$refs.cropper.cropper.getCanvasData();
    },
    setCanvasData(data) {
      this.$refs.cropper.cropper.setCanvasData(data);
    },
    getCropBoxData() {
      return this.$refs.cropper.cropper.getCropBoxData();
    },
    setCropBoxData(data) {
      this.$refs.cropper.cropper.setCropBoxData(data);
    },
    getCroppedCanvas(options = {}) {
      return this.$refs.cropper.cropper.getCroppedCanvas(options);
    },
    setAspectRatio(aspectRatio) {
      this.$refs.cropper.cropper.setAspectRatio(aspectRatio);
    },
    setDragMode(mode = "none") {
      this.$refs.cropper.cropper.setDragMode(mode);
    },
    setViewMode(vm = 0) {
      (this.$refs.cropper.cropper) ? this.destroy() : null;
      let options = this.options();
      options.viewMode = vm;
      this.init(options);
    }
  }
};
</script>

<style scoped>
.img {
  max-width: 100%;
}
</style>