/**
    API:
    toggleSee   是否显示隐藏/显示密码切换按钮   默认false   Boolean
    placeholder 同input的placeholder   String
    id          当前组件的id(必须)   Number|String
    value       父元素的绑定值   仅做显示用   Number|String
    maxlength   最大输入长度   不设置则不限制   设置为负数则不可输入, Number|String
    type        输入类型   默认为password   可选项为number   String
    caretColor  光标颜色   默认#333     String
    disabled    是否禁用    默认false   Boolean
    showClear   是否显示清除按钮    默认true    Boolean
    limit       限制类型    可选number|text, text限制为仅限输入字母、数字、下划线      Boolean
    

    Event:
    change      每次输入时返回当前全部输入值和id   可直接替换使用     返回值:(value,id)
    clear       点击清除键时触发,返回当前组件的id       返回值:(id)
 */
<template>
	<div class="PwdInputXCY">
		<div class="inputDiv">
			<input
				:type="(showPwd||type=='number')?(type=='number'?'number':'text'):'password'"
				:placeholder="placeholder"
				:value="value"
				@input="onInput"
				pattern="\d*"
				:disabled="disabled==true"
				:style="'caret-color:'+caretColor+';'"
			>
			<img
				tapmode
				@click="clear"
				v-show="value.length>0&&showClear"
				src="../static/img/cancel.png"
			>
			<span class="split" v-show="toggleSee&&value.length>0"></span>
			<img
				v-show="toggleSee&&!showPwd&&value.length>0"
				tapmode
				@click="toggleHide"
				src="../static/img/hide.png"
			>
			<img
				v-show="toggleSee&&showPwd&&value.length>0"
				@click="toggleHide"
				tapmode
				src="../static/img/hide2.png"
			>
		</div>
	</div>
</template>

<script>
export default {
	props: {
		toggleSee: {
			default: false
		},
		placeholder: {
			default: ""
		},
		id: {
			default: ""
		},
		value: {
			default: ""
		},
		maxlength: {
			default: ""
		},
		type: {
			default: "password"
		},
		caretColor: {
			default: "#333;"
		},
		disabled: {
			default: false
		},
		showClear: {
			default: true
		},
		limit: ""
	},
	data() {
		return {
			showPwd: false
		};
	},
	mounted() {
		if (!this.id) {
			console.error(
				"id属性为必传项,便于您在于同时存在多个组件时区分它们"
			);
		}
		if (this.type != "password" && this.type != "number") {
			console.error(
				"type属性仅支持password和number两个值,默认为password"
			);
		}
	},
	methods: {
		onInput(e) {
			let key = e.target.value;
			if (this.limit == "number") {
				let numReg = /\D/g;
				key = key.replace(numReg, "");
				e.target.value = key;
			}
			if (this.limit == "text") {
				let numReg = /\W/g;
				key = key.replace(numReg, "");
				e.target.value = key;
			}
			if (this.maxlength && key.length > this.maxlength) {
				e.target.value = key.substr(0, this.maxlength);
				return;
			}
			key = e.target.value;
			console.log(key);
			this.$emit("change", key, this.id);
		},
		toggleHide() {
			this.showPwd = !this.showPwd;
		},
		clear() {
			console.log("..11");
			this.$emit("clear", this.id);
		}
	}
};
</script>

<style lang="less">
.PwdInputXCY {
	.inputDiv {
		background: #fff;
		border-bottom: 1px solid #eaeaea;
		height: 17vw;
		display: flex;
		align-items: center;
		input {
			flex: 1;
			width: 100%;
			outline: none;
			height: 100%;
			border: none;
		}
		img {
			width: 4.5vw;
		}
		.split {
			width: 1px;
			height: 3vw;
			background: #dcdcdc;
			margin: 0 3vw;
		}
	}
}
</style>