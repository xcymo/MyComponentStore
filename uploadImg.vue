/**
 这是用于上传图片的组件，是基于Vant的uploader组件编写的
 说明：
 1、需要在父组件定义如下两个方法
        changeImgArr(file) {
			if (file.file) {
				this.imgArr = this.imgArr.concat([file]);
			} else {
				this.imgArr = this.imgArr.concat(file);
			}
		},
		deleteImg(index) {
			this.imgArr.splice(index, 1);
		}
    其中第一个方法用于上传图片，第二个用于删除图片
2、删除和上传按钮是我本地的图片，需替换
3、关于调用参数等：
    imgArr是父组件需要向子组件传递的对象
    changeImgArr和deleteImg是父组件接受子组件向上传递事件的两个方法
4、返回值说明：imgArr是一个数组，其中对象的形式为
    {
        content:base64编码,
        file:文件其他信息            
    }
5、具体用例
    <UploadImg
        :imgArr="imgArr"
        @changeImgArr="changeImgArr"
        @deleteImg="deleteImg"
    />
 */

<template>
	<div class="UploadImgXCY">
		<div class="imgLoop">
			<div class="imgBox" v-for="(item,index) in imgArr" :key="index">
				<img class="loadedImg" :src="item.content">
				<!-- 此图src为本地，请替换 -->
				<img
					class="clear"
					@click="deleteImg(index)"
					src="../assets/img/clear.png"
					alt
				>
			</div>
			<van-uploader
				:after-read="onRead"
				accept="image/gif, image/jpeg, image/png, image/jpg"
				multiple
			>
				<!-- 此图src为本地，请替换 -->
				<img class="clickIcon" src="../assets/img/addImg.png">
			</van-uploader>
		</div>
	</div>
</template>

<script>
export default {
	props: ["imgArr"],
	mounted() {
		console.log(typeof this.imgArr);
		if (typeof this.imgArr != "object") {
			console.error("imgArr属性为必传项");
		}
	},
	methods: {
		onRead(file) {
			this.$emit("changeImgArr", file);
		},
		deleteImg(index) {
			this.$emit("deleteImg", index);
		}
	}
};
</script>

<style lang="less">
.UploadImgXCY {
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