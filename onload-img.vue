<template>
	<view class="uni-list list-pd">
	    <view class="uni-list-cell cell-pd">
	        <view class="uni-uploader">
	            <view class="uni-uploader-head">
	                <view class="uni-uploader-title">点击可预览选好的图片</view>
	                <view class="uni-uploader-info">{{imageList.length}}/9</view>
	            </view>
	            <view class="uni-uploader-body">
	                <view class="uni-uploader__files">
	                    <block v-for="(image,index) in imageList" :key="index">
	                        <view class="uni-uploader__file">
	                            <image class="uni-uploader__img" :src="image" :data-src="image" @tap="previewImage"></image>
								<image @click="delect(index)" class="onloadimg" src="../../static/shanchu.png" mode=""></image>
	                        </view>
	                    </block>
	                    <view class="uni-uploader__input-box">
	                        <view class="uni-uploader__input" @tap="chooseImage"></view>
	                    </view>
	                </view>
	            </view>
	        </view>
	    </view>
	</view>
</template>

<script>
	// #ifdef APP-PLUS
	import permision from "@/common/permission.js"
	// #endif
	var sourceType = [
	    ['camera'],
	    ['album'],
	    ['camera', 'album']
	]
	var sizeType = [
	    ['compressed'],
	    ['original'],
	    ['compressed', 'original']
	]
	export default {
		data() {
			return {
				imageList: [],
				sourceTypeIndex: 2,
				sourceType: ['拍照', '相册', '拍照或相册'],
				sizeTypeIndex: 2,
				sizeType: ['压缩', '原图', '压缩或原图'],
				countIndex: 8,
				count: [1, 2, 3, 4, 5, 6, 7, 8, 9]
			}
		},
		methods: {
			chooseImage: async function() {
			    // #ifdef APP-PLUS
			    // TODO 选择相机或相册时 需要弹出actionsheet，目前无法获得是相机还是相册，在失败回调中处理
			    if (this.sourceTypeIndex !== 2) {
			        let status = await this.checkPermission();
			        if (status !== 1) {
			            return;
			        }
			    }
			    // #endif
			
			    if (this.imageList.length === 9) {
			       return;
			    }
			    uni.chooseImage({
			        sourceType: sourceType[this.sourceTypeIndex],
			        sizeType: sizeType[this.sizeTypeIndex],
			        count: this.imageList.length + this.count[this.countIndex] > 9 ? 9 - this.imageList.length :
			            this.count[this.countIndex],
			        success: (res) => {
			            this.imageList = this.imageList.concat(res.tempFilePaths);
						this.$emit('upload',this.imageList)
			        },
			        fail: (err) => {
			            // #ifdef APP-PLUS
			            if (err['code'] && err.code !== 0 && this.sourceTypeIndex === 2) {
			                this.checkPermission(err.code);
			            }
			            // #endif
			        }
			    })
			},
			previewImage: function(e) {
			    var current = e.target.dataset.src
			    uni.previewImage({
			        current: current,
			        urls: this.imageList
		    })
			},
			async checkPermission(code) {
			    let type = code ? code - 1 : this.sourceTypeIndex;
			    let status = permision.isIOS ? await permision.requestIOS(sourceType[type][0]) :
			        await permision.requestAndroid(type === 0 ? 'android.permission.CAMERA' :
			            'android.permission.READ_EXTERNAL_STORAGE');
			
			    if (status === null || status === 1) {
			        status = 1;
			    } else {
			        uni.showModal({
			            content: "没有开启权限",
			            confirmText: "设置",
			            success: function(res) {
			                if (res.confirm) {
			                    permision.gotoAppSetting();
								
			                }
			            }
			        })
			    }
			
			    return status;
			},
			// 删除图片
			delect(index){
				uni.showModal({
				    title: '提示',
				    content: '是否要删除该图片',
				    success:(res)=>{
				        if (res.confirm) {
				          this.imageList.splice(index,1);
						  this.$emit('upload',this.imageList)
				        } else if (res.cancel) {
				            console.log('用户点击取消');
				        }
				    }
				});
			}
		}
	}
</script>

<style scoped>
  .cell-pd {
        padding: 22upx 30upx;
    }

    .list-pd {
        margin-top: 50upx;
    }
	.onloadimg{
		width: 20px;
		height: 20px;
		position: absolute;
		top: 5px;
		right: 5px;
	}
	.uni-uploader__file{
		position: relative;
	}
</style>
