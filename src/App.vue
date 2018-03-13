<template>
	<div id="app">
		<div class="container">
			<br><br>
			<div class="row">
				<div class="left col-md-9">
					<div class="workbench col-md-12">
						<vue-cropper :src="require('./assets/test.jpg')" ref="crop" height="425"></vue-cropper>
					</div>
					<br>
					<div class="buttons col-md-12">
						<div class="one">
							<div class="btn-group"><span class="btn btn-primary fas fa-arrows-alt"/><span class="btn btn-primary fas fa-crop"/></div>
							<div class="btn-group"><span class="btn btn-primary fas fa-search-plus"/><span class="btn btn-primary fas fa-search-minus"/></div>
							<div class="btn-group"><span class="btn btn-primary fas fa-arrow-left"/><span class="btn btn-primary fas fa-arrow-right"/><span class="btn btn-primary fas fa-arrow-up"/><span class="btn btn-primary fas fa-arrow-down"/></div>
							<div class="btn-group"><span class="btn btn-primary fas fa-undo-alt"/><span class="btn btn-primary fas fa-redo-alt"/></div>
							<div class="btn-group"><span class="btn btn-primary fas fa-arrows-alt-h"/><span class="btn btn-primary fas fa-arrows-alt-v"/></div>
							<div class="btn-group"><span class="btn btn-primary fas fa-check"/><span class="btn btn-primary fas fa-times"/></div>
							<div class="btn-group"><span class="btn btn-primary fas fa-sync-alt"/><span class="btn btn-primary fas fa-upload"/><span class="btn btn-primary fas fa-power-off"/></div>
						</div>
						<div class="two">
							<div class="btn-group">
								<span class="btn btn-success">Get Croped Canvas</span>
								<span class="btn btn-success">160x90</span>
								<span class="btn btn-success">320x180</span>
							</div>
							<span class="btn btn-secondary">Get Data</span>
							<span class="btn btn-secondary">Set Data</span>
							<span class="btn btn-secondary">Get Container Data</span>
							<span class="btn btn-secondary">Get Image Data</span>
							<span class="btn btn-secondary">Get Canvas Data</span>
						</div>
						<div class="three">
							<span class="btn btn-secondary">Set Canvas Data</span>
							<span class="btn btn-secondary">Get Crop Box Data</span>
							<span class="btn btn-secondary">Set Crop Box Data</span>
							<span class="btn btn-secondary">Move to [0,0]</span>
							<span class="btn btn-secondary">Zoom to 100%</span>
							<span class="btn btn-secondary">Rotate 180º</span>
							<span class="btn btn-secondary">Scale (-2, -1)</span>
						</div>
						<div class="four">
							<textarea name="" id="" cols="130" rows="1" placeholder="Get data to here or set data with this value"></textarea>
						</div>
					</div>
				</div>
				<div class="right col-md-3">
					<div class="previews">
						<img src="" alt="" class="col-md-12">
						<img src="" alt="" class="col-md-4">
						<img src="" alt="" class="col-md-4">
						<img src="" alt="" class="col-md-4">
					</div>
					<div class="inputs">
						<div class="input-group">
							<div class="input-group-prepend"><span class="input-group-text">X</span></div>
							<input type="text" class="form-control">
							<div class="input-group-append"><span class="input-group-text">px</span></div>
						</div>
						<div class="input-group">
							<div class="input-group-prepend"><span class="input-group-text">Y</span></div>
							<input type="text" class="form-control">
							<div class="input-group-append"><span class="input-group-text">px</span></div>
						</div>
						<div class="input-group">
							<div class="input-group-prepend"><span class="input-group-text">Width</span></div>
							<input type="text" class="form-control">
							<div class="input-group-append"><span class="input-group-text">px</span></div>
						</div>
						<div class="input-group">
							<div class="input-group-prepend"><span class="input-group-text">Height</span></div>
							<input type="text" class="form-control">
							<div class="input-group-append"><span class="input-group-text">px</span></div>
						</div>
						<div class="input-group">
							<div class="input-group-prepend"><span class="input-group-text">Rotate</span></div>
							<input type="text" class="form-control">
							<div class="input-group-append"><span class="input-group-text">deg</span></div>
						</div>
						<div class="input-group">
							<div class="input-group-prepend"><span class="input-group-text">ScaleX</span></div>
							<input type="text" class="form-control">
						</div>
						<div class="input-group">
							<div class="input-group-prepend"><span class="input-group-text">ScaleY</span></div>
							<input type="text" class="form-control">
						</div>
					</div>
					<div class="controllers"></div>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
import VueCroper from "./components/VueCropper";

export default {
  name: "App",
  components: {
    "vue-cropper": VueCroper
  },
  methods: {
    myImage() {
      console.log(this.$refs.crop.getCroppedCanvas().toDataURL());
    }
  }
};
</script>

<style>
.buttons > div {
  padding-bottom: 5px;
}
.inputs .input-group {
  padding-bottom: 5px;
}
</style>
