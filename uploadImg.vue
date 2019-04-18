/**
    API
    imgArr          父组件存放图片的数组,需自己处理,必传项,Object

    Event
    changeImgArr    选择照片时触发此方法,返回值为一个对象,{content:base64编码, file:文件其他信息}
    deleteImg       删除照片时触发,返回被删除照片的下标

    例子:
    <UploadImg
        :imgArr="imgArr"
        @changeImgArr="changeImgArr"
        @deleteImg="deleteImg"
    />

    备注:
    需自己替换此组件中的两个img标签中的图片(选择照片、删除照片对应的图标)
 */
<template>
	<div class="UploadImgXCY">
		<div class="imgLoop">
			<div class="imgBox" v-for="(item,index) in imgArr" :key="index">
				<img class="loadedImg" :src="item.content">
				<img class="clear" @click="deleteImg(index)" src="../static/img/clear.png">
			</div>

			<label class="uploader_box" for="logimg">
				<input type="file" id="logimg" accept="image/*" @change="onRead">
				<img class="clickIcon" src="../static/img/addImg.png">
			</label>

		</div>
	</div>
</template>

<script>
export default {
	props: {
		imgArr: ''
	},
	mounted() {
		if (typeof this.imgArr != "object") {
			console.error("imgArr属性为必传项");
		}
	},
	methods: {
		onRead(file) {
			let file2 = document.getElementById("logimg").files;
			let file3 = {};
			let reader = new FileReader();

			file3 = [];
			for (let i of file2) {
				reader.readAsDataURL(i);
				reader.onload = res => {
					file3 = {
						content: res.currentTarget.result,
						file: i
					};
					this.$emit("changeImgArr", file3);
				};
			}
			// this.$emit("changeImgArr", file);
		},
		deleteImg(index) {
			this.$emit("deleteImg", index);
		}
	}
};
</script>

<style lang="less">
.UploadImgXCY {
	.uploader_box {
		height: auto;
		overflow: hidden;
		position: relative;
	}
	#logimg {
		display: none;
	}
	.clickIcon {
		width: 100%;
		height: auto;
	}
	.clickIcon {
		width: 19.2vw;
		height: 19.2vw;
		margin: 0 2.4vw;
	}
	.imgLoop {
		display: flex;
		flex-wrap: wrap;
		.imgBox {
			position: relative;
			.loadedImg {
				width: 19.2vw;
				height: 19.2vw;
				margin: 0 2.4vw;
			}
			.clear {
				position: absolute;
				right: 0;
				top: -2vw;
				width: 5.2vw;
				height: 5.2vw;
			}
		}
	}
}
</style>