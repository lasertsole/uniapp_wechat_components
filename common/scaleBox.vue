<template>
	<view class="scaleBox">
		<slot name="default"></slot>
	</view>
</template>

<script lang="ts" setup>
	import { ref, defineProps, onMounted, getCurrentInstance } from "vue"
	const props:any = defineProps({
		isBaseWidth:{type:Boolean, required:false, default:true},
		baseSize:{type:String, required:true},
		changeScale:{type:Number, required:true},
	});
	
	function computeBoxParam(isBaseWidth:boolean, baseSize:string, changeScale:number):string{//等比改变函数
		let sizeNum:number = parseFloat(baseSize.match(/[0-9]+/g)[0]);
		let sizeString:string = baseSize.match(/([a-z]|[A-Z])+/g)[0];
		return sizeNum*changeScale+sizeString;
	}
	
	const query:any = uni.createSelectorQuery().in(getCurrentInstance());//创建uniapp节点选择器
	const scaleBoxSize = ref<string>("");
	onMounted(()=>{
		query.select('.scaleBox').boundingClientRect(data => {//获取导航栏总高度
			scaleBoxSize.value = data?.width + "px";
		}).exec();
		computeBoxParam(props.isBaseWidth, scaleBoxSize.value, props.changeScale);
	})
</script>

<style lang="scss" scoped>
	.scaleBox{
		background-color: aqua;//
		$baseSize: v-bind(baseSize);
		$isBaseWidth: v-bind(isBaseWidth);
		@if $isBaseWidth{
			min-width: $baseSize;
			max-width: $baseSize;
		}
		@else{
			min-height: $baseSize;
			max-height: $baseSize;
		}
	}
</style>