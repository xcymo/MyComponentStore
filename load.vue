/**
这是基于Vant的组件,使用前请确保安装了Vant,并Use了Loading组件

    API
    opacity         组件透明度,默认0.3,String|Number
    boxSize         方框的尺寸,默认20vw,所有单位可选(px、vw、vh、%、rem等),String
    type            loading类型,同Vant的Loading组件,默认circular,可选spinner,String
    color           loading颜色,同Vant,默认#c9c9c9,String
    size            loading尺寸,同Vant,默认30px,String
    show            是否显示此组件,默认false,Boolean
    position        组件在页面的位置,基于定位,要用这个属性须确保外层无相对定位的父元素,可选值center,无默认值,String
    modal           是否存在遮罩层,默认false,可选true,Boolean
    modalOpacity    遮罩层的透明度,默认为0.2,Number
 */
<template>
	<div class="LoadXCY" v-show="show" :class="position=='center'?'center':''">
		<div class="modal" v-show="modal" :style="'opacity:'+modalOpacity+';'"></div>
		<div
			class="loadContain"
			:style="'opacity:'+opacity+';width:'+size+';height:'+size+';padding:calc('+boxSize+'/2 - '+size+'/2)'"
		>
			<van-loading :type="type" :color="color" :size="size"/>
		</div>
	</div>
</template>

<script>
export default {
	props: {
		opacity: {
			default: "0.3"
		},
		boxSize: {
			default: "20vw"
		},
		type: {
			default: "circular"
		},
		color: {
			default: "#FFF"
		},
		size: {
			default: "10vw"
		},
		show: {
			default: false
		},
		position: {
			default: ""
		},
		modal: {
			default: false
		},
		modalOpacity: {
			default: 0.2
		}
	},
	mounted() {
		if (this.position != "center") {
			console.warn(
				"position 属性仅支持center, 你传递的值为:" + this.position
			);
		}
		if (isNaN(Number(this.opacity)) || this.opacity < 0) {
			console.warn("opacity 值有误,请检查, 你传递的值为:" + this.opacity);
		}
		if (typeof this.show != "boolean") {
			console.warn("show 接受一个布尔值,你传递的值为:" + this.show);
		}
	}
};
</script>

<style lang="less">
.LoadXCY {
	text-align: center;
	.modal {
		position: fixed;
		left: 0;
		right: 0;
		top: 0;
		bottom: 0;
		z-index: 99;
		background: rgb(0, 0, 0);
	}
	.loadContain {
		display: inline-block;
		text-align: center;
		border-radius: 8px;
		background-color: #000;
	}
}
.center.LoadXCY {
	position: absolute;
	left: 0;
	right: 0;
	top: calc(50% - 10vw);
}
</style>