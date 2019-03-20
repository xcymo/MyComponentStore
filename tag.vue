/**
    标签组件

    API
    text    标签内部文字，String
    color   标签主题颜色，默认#EC6464，String
    size    标签字体大小，默认2.9vw，String
    height  标签高度，默认4vw，String
 */
<template>
	<span
		class="TagXCY"
		:style="'color:'+color+';line-height:'+height+';border:1px solid '+color+';font-size:'+size+';'"
	>{{text}}</span>
</template>

<script>
export default {
	props: {
		text: {
			default: "标签文字"
		},
		color: {
			default: "#EC6464"
		},
		height: {
			default: "4vw"
		},
		size: {
			default: "2.9vw"
		}
	}
};
</script>

<style lang="less">
.TagXCY {
	padding: 0 1.5vw;
	display: inline-block;
	border-radius: 4px;
}
</style>