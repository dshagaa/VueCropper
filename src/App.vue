<template>
	<div id="app">
		<div class="container">
			<br><br>
			<div class="row">
				<div class="left col-md-9">
					<div class="workbench col-md-12">
						<vue-cropper :src="image.source" ref="crop" height="425"></vue-cropper>
					</div>
					<br>
					<div class="buttons col-md-12">
						<div class="one">
							<div class="btn-group"><span class="btn btn-primary fas fa-arrows-alt" @click="$refs.crop.setDragMode('move')"/><span class="btn btn-primary fas fa-crop" @click="$refs.crop.setDragMode('crop')"/></div>
							<div class="btn-group"><span class="btn btn-primary fas fa-search-plus" @click="$refs.crop.zoomFn(0.1)"/><span class="btn btn-primary fas fa-search-minus" @click="$refs.crop.zoomFn(-0.1)"/></div>
							<div class="btn-group"><span class="btn btn-primary fas fa-arrow-left" @click="$refs.crop.move(-10,0)"/><span class="btn btn-primary fas fa-arrow-right" @click="$refs.crop.move(10,0)"/><span class="btn btn-primary fas fa-arrow-up" @click="$refs.crop.move(0,-10)"/><span class="btn btn-primary fas fa-arrow-down" @click="$refs.crop.move(0,10)"/></div>
							<div class="btn-group"><span class="btn btn-primary fas fa-undo-alt" @click="$refs.crop.rotate(-10)"/><span class="btn btn-primary fas fa-redo-alt" @click="$refs.crop.rotate(10)"/></div>
							<div class="btn-group"><span class="btn btn-primary fas fa-arrows-alt-h" @click="invert('X')"/><span class="btn btn-primary fas fa-arrows-alt-v" @click="invert('Y')"/></div>
							<div class="btn-group"><span class="btn btn-primary fas fa-check" @click="$refs.crop.cropFn()"/><span class="btn btn-primary fas fa-times" @click="$refs.crop.clear()"/></div>
							<div class="btn-group">
								<span class="btn btn-primary fas fa-sync-alt" @click="$refs.crop.reset()"/>
								<label for="inputImage" class="btn btn-primary">
								<span class="fas fa-upload"/>
								</label>
								<span class="btn btn-primary fas fa-power-off" @click="$refs.crop.destroy()"/>
								<input id="inputImage" @change="upload" type="file" accept=".jpg,.jpeg,.png,.gif,.bmp,.tiff" hidden>
							</div>
						</div>
						<div class="two">
							<div class="btn-group">
								<span class="btn btn-success" @click="getImage">Get Croped Canvas</span>
								<span class="btn btn-success" @click="getImage(160,90)">160x90</span>
								<span class="btn btn-success" @click="getImage(320,180)">320x180</span>
							</div>
							<span class="btn btn-secondary" @click="getData('getData')">Get Data</span>
							<span class="btn btn-secondary" @click="setData('setData')">Set Data</span>
							<span class="btn btn-secondary" @click="getData('getContainerData')">Get Container Data</span>
						</div>
						<div class="three">
							<span class="btn btn-secondary" @click="getData('getImageData')">Get Image Data</span>
							<span class="btn btn-secondary" @click="getData('getCanvasData')">Get Canvas Data</span>
							<span class="btn btn-secondary" @click="setData('setCanvasData')">Set Canvas Data</span>
							<span class="btn btn-secondary" @click="getData('getCropBoxData')">Get Crop Box Data</span>
							<span class="btn btn-secondary" @click="setData('setCropBoxData')">Set Crop Box Data</span>
						</div>
						<div class="four">
							<span class="btn btn-secondary" @click="set('moveTo',0,0)">Move to [0,0]</span>
							<span class="btn btn-secondary" @click="set('zoomTo',1)">Zoom to 100%</span>
							<span class="btn btn-secondary" @click="set('rotateTo',180)">Rotate 180º</span>
							<span class="btn btn-secondary" @click="set('scale',-2,-1)">Scale (-2, -1)</span>
						</div>
						<div class="five">
							<textarea v-model="textArea" rows="1" placeholder="Get data to here or set data with this value"></textarea>
						</div>
					</div>
				</div>
				<div class="right col-md-3">
					<div class="previews">
						<img :src="image.preview" alt="" class="" height="144" width="256">
						<img :src="image.preview" alt="" class="" height="72" width="128">
						<img :src="image.preview" alt="" class="" height="36" width="64">
						<img :src="image.preview" alt="" class="" height="18" width="32">
					</div>
					<div class="inputs">
						<div class="input-group">
							<div class="input-group-prepend"><span class="input-group-text">X</span></div>
							<input type="text" class="form-control" v-model="inputs.x">
							<div class="input-group-append"><span class="input-group-text">px</span></div>
						</div>
						<div class="input-group">
							<div class="input-group-prepend"><span class="input-group-text">Y</span></div>
							<input type="text" class="form-control" v-model="inputs.y">
							<div class="input-group-append"><span class="input-group-text">px</span></div>
						</div>
						<div class="input-group">
							<div class="input-group-prepend"><span class="input-group-text">Width</span></div>
							<input type="text" class="form-control" v-model="inputs.w">
							<div class="input-group-append"><span class="input-group-text">px</span></div>
						</div>
						<div class="input-group">
							<div class="input-group-prepend"><span class="input-group-text">Height</span></div>
							<input type="text" class="form-control" v-model="inputs.h">
							<div class="input-group-append"><span class="input-group-text">px</span></div>
						</div>
						<div class="input-group">
							<div class="input-group-prepend"><span class="input-group-text">Rotate</span></div>
							<input type="text" class="form-control" v-model="inputs.r">
							<div class="input-group-append"><span class="input-group-text">deg</span></div>
						</div>
						<div class="input-group">
							<div class="input-group-prepend"><span class="input-group-text">ScaleX</span></div>
							<input type="text" class="form-control" v-model="inputs.sX">
						</div>
						<div class="input-group">
							<div class="input-group-prepend"><span class="input-group-text">ScaleY</span></div>
							<input type="text" class="form-control" v-model="inputs.sY">
						</div>
					</div><br>
					<div class="controllers">
						<div class="btn-group">
							<span class="btn btn-primary" 
							v-for="(ratio,index) in controllers.aspectRatioList" 
							@click="setAspectRatio(ratio.value)"
							:key="index">{{ratio.label}}</span>
						</div>
						<div class="btn-group">
							<span class="btn btn-primary"
							v-for="(vM,index) in controllers.viewMode"
							:key="index"
							@click="setViewMode(vM.value)">{{vM.label}}</span>
						</div>
						<div class="btn-group dropdown">
							<button data-toggle="dropdown" class="btn btn-primary dropdown-toggle">
								Toggle Options
							</button>
							<ul class="dropdown-menu">
								<li class="dropdown-item">
								<div class="form-check">
									<input class="form-check-input" id="responsive" name="responsive" checked="" type="checkbox">
									<label class="form-check-label" for="responsive">responsive</label>
								</div>
								</li>
								<li class="dropdown-item">
								<div class="form-check">
									<input class="form-check-input" id="restore" name="restore" checked="" type="checkbox">
									<label class="form-check-label" for="restore">restore</label>
								</div>
								</li>
								<li class="dropdown-item">
								<div class="form-check">
									<input class="form-check-input" id="checkCrossOrigin" name="checkCrossOrigin" checked="" type="checkbox">
									<label class="form-check-label" for="checkCrossOrigin">checkCrossOrigin</label>
								</div>
								</li>
								<li class="dropdown-item">
								<div class="form-check">
									<input class="form-check-input" id="checkOrientation" name="checkOrientation" checked="" type="checkbox">
									<label class="form-check-label" for="checkOrientation">checkOrientation</label>
								</div>
								</li>
								<li class="dropdown-item">
								<div class="form-check">
									<input class="form-check-input" id="modal" name="modal" checked="" type="checkbox">
									<label class="form-check-label" for="modal">modal</label>
								</div>
								</li>
								<li class="dropdown-item">
								<div class="form-check">
									<input class="form-check-input" id="guides" name="guides" checked="" type="checkbox">
									<label class="form-check-label" for="guides">guides</label>
								</div>
								</li>
								<li class="dropdown-item">
								<div class="form-check">
									<input class="form-check-input" id="center" name="center" checked="" type="checkbox">
									<label class="form-check-label" for="center">center</label>
								</div>
								</li>
								<li class="dropdown-item">
								<div class="form-check">
									<input class="form-check-input" id="highlight" name="highlight" checked="" type="checkbox">
									<label class="form-check-label" for="highlight">highlight</label>
								</div>
								</li>
								<li class="dropdown-item">
								<div class="form-check">
									<input class="form-check-input" id="background" name="background" checked="" type="checkbox">
									<label class="form-check-label" for="background">background</label>
								</div>
								</li>
								<li class="dropdown-item">
								<div class="form-check">
									<input class="form-check-input" id="autoCrop" name="autoCrop" checked="" type="checkbox">
									<label class="form-check-label" for="autoCrop">autoCrop</label>
								</div>
								</li>
								<li class="dropdown-item">
								<div class="form-check">
									<input class="form-check-input" id="movable" name="movable" checked="" type="checkbox">
									<label class="form-check-label" for="movable">movable</label>
								</div>
								</li>
								<li class="dropdown-item">
								<div class="form-check">
									<input class="form-check-input" id="rotatable" name="rotatable" checked="" type="checkbox">
									<label class="form-check-label" for="rotatable">rotatable</label>
								</div>
								</li>
								<li class="dropdown-item">
								<div class="form-check">
									<input class="form-check-input" id="scalable" name="scalable" checked="" type="checkbox">
									<label class="form-check-label" for="scalable">scalable</label>
								</div>
								</li>
								<li class="dropdown-item">
								<div class="form-check">
									<input class="form-check-input" id="zoomable" name="zoomable" checked="" type="checkbox">
									<label class="form-check-label" for="zoomable">zoomable</label>
								</div>
								</li>
								<li class="dropdown-item">
								<div class="form-check">
									<input class="form-check-input" id="zoomOnTouch" name="zoomOnTouch" checked="" type="checkbox">
									<label class="form-check-label" for="zoomOnTouch">zoomOnTouch</label>
								</div>
								</li>
								<li class="dropdown-item">
								<div class="form-check">
									<input class="form-check-input" id="zoomOnWheel" name="zoomOnWheel" checked="" type="checkbox">
									<label class="form-check-label" for="zoomOnWheel">zoomOnWheel</label>
								</div>
								</li>
								<li class="dropdown-item">
								<div class="form-check">
									<input class="form-check-input" id="cropBoxMovable" name="cropBoxMovable" checked="" type="checkbox">
									<label class="form-check-label" for="cropBoxMovable">cropBoxMovable</label>
								</div>
								</li>
								<li class="dropdown-item">
								<div class="form-check">
									<input class="form-check-input" id="cropBoxResizable" name="cropBoxResizable" checked="" type="checkbox">
									<label class="form-check-label" for="cropBoxResizable">cropBoxResizable</label>
								</div>
								</li>
								<li class="dropdown-item">
								<div class="form-check">
									<input class="form-check-input" id="toggleDragModeOnDblclick" name="toggleDragModeOnDblclick" checked="" type="checkbox">
									<label class="form-check-label" for="toggleDragModeOnDblclick">toggleDragModeOnDblclick</label>
								</div>
								</li>
							</ul>
						</div>
					</div>
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
		getImage(w, h) {
			this.image.preview = this.$refs.crop.getCroppedCanvas({width:w,height:h}).toDataURL();
		},
		setAspectRatio(value) {
			this.$refs.crop.setAspectRatio(value);
		},
		setViewMode(value) {
			this.$refs.crop.setViewMode(value);
		},
		invert(e) {
			this.$refs.crop['scale'+e](this.inputs['s'+e]*-1);
			let data = this.$refs.crop.getImageData();
			let key = 's'+e;
			this.inputs[key] = data['scale'+e];
		},
		getData(fn, param = []) {
			let r = (this.$refs.crop[fn])();
			this.textArea = JSON.stringify(r);
		},
		setData(fn) {
			let data = JSON.parse(this.textArea);
			this.$refs.crop[fn](data);
		},
		set(fn, param1 = 0, param2 = 0) {
			this.$refs.crop[fn](param1,param2);
		},
		upload(e) {
			let files = e.target.files;
			let file;
			let uploadedImageURL = '', uploadedImageName, uploadedImageType;
			let URL = window.URL;

			if (this.$refs.crop && files && files.length) {
				file = files[0];

				if (/^image\/\w+/.test(file.type)) {
					uploadedImageType = file.type;
					uploadedImageName = file.name;

					console.log(uploadedImageURL)
					if (uploadedImageURL) {
						URL.revokeObjectURL(uploadedImageURL);
					}

					this.image.source = uploadedImageURL = URL.createObjectURL(file);
					this.$refs.crop.replace(this.image.source);
					e.target.value = null;
				} else {
					window.alert('Please choose an image file.');
				}
			}
		}
	},
	data(){
		return {
			image: {
				source: require('./assets/test.jpg'),
				preview: '',
				miniatures: {
					lg: '',
					md: '',
					sm: '',
					xs: ''
				}
			},
			textArea: '',
			inputs: {
				x: '',
				y: '',
				w: '',
				h: '',
				r: '',
				sX: 1,
				sY: 1
			},
			controllers: {
				aspectRatioList: [
					{label: '16:9', value: 16/9},
					{label: '4:3', value: 4/3},
					{label: '1:1', value: 1},
					{label: '2:3', value: 2/3},
					{label: 'Free', value: NaN}
				],
				viewMode: [
					{label: 'VM0', value: 0},
					{label: 'VM1', value: 1},
					{label: 'VM2', value: 2},
					{label: 'VM3', value: 3}
				],
				options: {
					responsive: true,
					restore: true,
					checkCrossOrigin: true,
					checkOrientation: true,
					modal: true,
					guides: true,
					center: true,
					highlight: true,
					background: true,
					autoCrop: true,
					movable: true,
					rotatable: true,
					scalable: true,
					zoomable: true,
					zoomOnTouch: true,
					zoomOnWheel: true,
					cropBoxMovable: true,
					cropBoxResizable: true,
					toggleDragModeOnDblclick: true
				}
			}
		}
	}
};
</script>

<style>
.buttons > div {
  padding-bottom: 5px;
}
.controllers .btn-group {
	padding-bottom: 5px;
}
.five textarea {
	width: 100%;
}
.inputs .input-group {
  padding-bottom: 5px;
}
.input-group-prepend .input-group-text {
	min-width: 5rem;
}
.input-group-append .input-group-text {
	min-width: 3.5rem;
}
.one .btn-group label {
	padding-top: 0;
	padding-bottom: 0;
	margin-top: 0;
	margin-bottom: 0;
}
</style>
