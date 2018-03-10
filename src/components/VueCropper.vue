<template>
	<div>
		<img :src="src" alt="" ref="cropper" width="15%">
	</div>
</template>
 
<script>
	import Cropper from 'cropperjs';
	export default {
		name:'vue-cropper',
		props: {
			autoCrop: {
				type: Boolean,
				default: false
			},
			src: {
				type: String,
				default: require('../assets/logo.png')
			}
		},
		created() {},
		beforeMount() {},
		mounted() {},
		methods: {
			init() {
				new Cropper(this.$refs.cropper,this.options)
			},
			crop() {
				this.$refs.cropper.crop();
			}
		},
		data() {
			return {
				options:{
					autoCrop: this.autoCrop
				}
			}
		}
	}
</script>

<style scoped>
</style>