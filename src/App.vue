<template>
	<div id="app">
		<div class="container">
			<br><br>
			<div class="row">
				<div class="left col-md-9">
					<div class="workbench col-md-12">
						<vue-cropper :src="image.source" :cropend="preview" :cropmove="preview" :ready="preview" ref="crop" height="425"></vue-cropper>
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
						<img :src="image.miniatures.lg" alt="" class="" width="256" height="144">
						<img :src="image.miniatures.md" alt="" class="" width="128" height="72">
						<img :src="image.miniatures.sm" alt="" class="" width="64" height="36">
						<img :src="image.miniatures.xs" alt="" class="" width="32" height="18">
					</div>
					<div class="inputs">
						<div class="input-group">
							<div class="input-group-prepend"><span class="input-group-text">{{inputs.x.label}}</span></div>
							<input type="text" class="form-control" v-model="inputs.x.value" @change="set(inputs.y.fn,parseInt(inputs.x.value),parseInt(inputs.y.value))">
							<div class="input-group-append"><span class="input-group-text">{{inputs.x.unidad}}</span></div>
						</div>
						<div class="input-group">
							<div class="input-group-prepend"><span class="input-group-text">{{inputs.y.label}}</span></div>
							<input type="text" class="form-control" v-model="inputs.y.value" @change="set(inputs.y.fn,parseInt(inputs.x.value),parseInt(inputs.y.value))">
							<div class="input-group-append"><span class="input-group-text">{{inputs.y.unidad}}</span></div>
						</div>
						<div class="input-group">
							<div class="input-group-prepend"><span class="input-group-text">{{inputs.w.label}}</span></div>
							<input type="text" class="form-control" v-model="inputs.w.value" @change="set(inputs.w.fn,{width:parseInt(inputs.w.value)})">
							<div class="input-group-append"><span class="input-group-text">{{inputs.w.unidad}}</span></div>
						</div>
						<div class="input-group">
							<div class="input-group-prepend"><span class="input-group-text">{{inputs.h.label}}</span></div>
							<input type="text" class="form-control" v-model="inputs.h.value" @change="set(inputs.h.fn,{height:parseInt(inputs.h.value)})">
							<div class="input-group-append"><span class="input-group-text">{{inputs.h.unidad}}</span></div>
						</div>
						<div class="input-group">
							<div class="input-group-prepend"><span class="input-group-text">{{inputs.r.label}}</span></div>
							<input type="text" class="form-control" v-model="inputs.r.value" @change="set(inputs.r.fn,parseInt(inputs.r.value))">
							<div class="input-group-append"><span class="input-group-text">{{inputs.r.unidad}}</span></div>
						</div>
						<div class="input-group">
							<div class="input-group-prepend"><span class="input-group-text">{{inputs.sX.label}}</span></div>
							<input type="text" class="form-control" v-model="inputs.sX.value" @change="set(inputs.sX.fn,parseInt(inputs.sX.value))">
						</div>
						<div class="input-group">
							<div class="input-group-prepend"><span class="input-group-text">{{inputs.sY.label}}</span></div>
							<input type="text" class="form-control" v-model="inputs.sY.value" @change="set(inputs.sY.fn,parseInt(inputs.sY.value))">
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
								<div class="form-check" v-for="(opt, index) in controllers.options" :key="index">
									<input class="form-check-input" :id="index" :name="index" type="checkbox" v-model="controllers.options[index]" @change="setOptions">
									<label class="form-check-label" :for="index">{{index}}</label>
								</div>
								</li>
							<!--
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
							-->
							</ul>
						</div>
					</div>
				</div>
			</div>
		</div>
		<div class="myModalC" ref="previewModal" v-show="modal">
			<div class="myModal">
				<span id="closeModal" @click="modal=false" class="fas fa-times btn-danger"></span>
				<img :src="image.preview" alt="">
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
	beforeUpdate() {
		this.get();
	},
	methods: {
		get() {
			let cnvDta = this.$refs.crop.getCanvasData();
			let imgDta = this.$refs.crop.getImageData();
			this.inputs.x.value = cnvDta.left;
			this.inputs.y.value = cnvDta.top;
			this.inputs.w.value = cnvDta.width;
			this.inputs.h.value = cnvDta.height;
			this.inputs.r.value = imgDta.rotate;
			this.inputs.sX.value = imgDta.scaleX;
			this.inputs.sY.value = imgDta.scaleY;
			
		},
		getData(fn, param = []) {
			let r = (this.$refs.crop[fn])();
			this.textArea = JSON.stringify(r);
		},
		getImage(w, h) {
			this.image.preview = this.$refs.crop.getCroppedCanvas({width:w,height:h}).toDataURL();
			this.modal = true;
		},
		set(fn, param1 = 0, param2 = 0) {
			this.$refs.crop[fn](param1,param2);
		},
		setAspectRatio(value) {
			this.$refs.crop.setAspectRatio(value);
		},
		setData(fn) {
			let data = JSON.parse(this.textArea);
			this.$refs.crop[fn](data);
		},
		setOptions(){
			let opt = {};
			for (const [key, value] of Object.entries(this.controllers.options)) {
				opt[key] = value;
			}
			this.$refs.crop.destroy();
			this.$refs.crop.init(opt);
		},
		setViewMode(value) {
			this.$refs.crop.setViewMode(value);
		},
		invert(e) {
			this.$refs.crop['scale'+e](this.inputs['s'+e].value*-1);
			let data = this.$refs.crop.getImageData();
			let key = 's'+e;
			this.inputs[key].value = data['scale'+e];
		},
		preview() {
			let crop = this.$refs.crop;
			this.image.miniatures.lg = crop.getCroppedCanvas({width:256,height:144}).toDataURL();
			this.image.miniatures.md = crop.getCroppedCanvas({width:128,height:72}).toDataURL();
			this.image.miniatures.sm = crop.getCroppedCanvas({width:64,height:36}).toDataURL();
			this.image.miniatures.xs = crop.getCroppedCanvas({width:32,height:18}).toDataURL();
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
			modal: false,
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
				x: {label: 'X', value: 0, fn: 'moveTo', unidad: 'px'},
				y: {label: 'Y', value: 0, fn: 'moveTo', unidad: 'px'},
				w: {label: 'Width', value: 0, fn: 'setCanvasData', unidad: 'px'},
				h: {label: 'Height', value: 0, fn: 'setCanvasData', unidad: 'px'},
				r: {label: 'Rotate', value: 0, fn: 'rotateTo', unidad: 'deg'},
				sX: {label: 'ScaleX', value: 1, fn: 'scaleX'},
				sY: {label: 'ScaleY', value: 1, fn: 'scaleY'}
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
.myModalC {
	width: 100vw;
	height: 100vh;;
	margin: 0;
	padding: 0;
	top: 0;
	left: 0;
	position: absolute;
	background-color: rgba(0, 0, 0, 0.7);
}
.myModal {
	width: 70vw;
	height: 40vh;
	margin: auto;
	top: 0;
	left: 0;
	bottom: 0;
	right: 0;
	position: fixed;
}
.myModal img {
	width: 100%;
	height: auto;
}
</style>
