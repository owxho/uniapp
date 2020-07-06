<template>
	<view class="content">
		<view class="header">
			<view class="header_cc">
				<view class="photolist">
					<!-- 上传图片 -->
					<view class="uni-uploader">
					    <view class="uni-uploader-body">
					        <view class="uni-uploader__files">
					            <block v-for="(image,index) in imglist1" :key="index">
					                <view class="uni-uploader__file">
										<view class="shanchu" @tap="shanchu(index)"></view>			
					                    <image class="uni-uploader__img" :src="image" :data-src="image" @tap="previewImage"></image>
					                </view>
					            </block>
					            <view class="uni-uploader__input-box">
					            </view>
					        </view>
					    </view>
					</view>
					<!-- 上传图片 -->
				</view>
			</view>
			<view class="header_bo">
				<view class="photo" @tap="chooseImage"></view>
				<view class="submit" @tap="tssubmit">提交</view>
			</view>
		</view>
	</view>
</template>

<script>
	// 上传图片
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
	// ---上传图片
	export default {
		components:{
		},
		data() {
			return {
				    // 上传图片
					imageList:[],
					sourceType: ['拍照', '相册', '拍照或相册'],
					sizeType: ['压缩', '原图', '压缩或原图'],
					imglist1:[],
					newsUrl:"http://mobile.seecitylife.com/api/web/index.php/"
			}
		},
		methods: {
			// 上传图片
			shanchu(index){
					uni.showModal({
					    title: '提示',
					    content: '确定要删除该照片吗',
					    success:  (res)=> {
							if(res.confirm){
								 this.imglist1.splice(index,1);
							}
					        
					    },
						
					});
				},	
			chooseImage: async function() {
				console.log(this.imglist1.length)
				    if (this.imglist1.length> 8) {
						uni.showToast({
							title:"图片数量超过限制",
							icon:"none"
						})
				        return;
				    }
				    uni.chooseImage({
				        count: 9, //默认9
				        sizeType: ['original', 'compressed'], //可以指定是原图还是压缩图，默认二者都有
				        sourceType: ['album','camera'], //从相册选择,
				        success: (res) => {
							console.log("res",res);
							this.imageList=[]
				            this.imageList = res.tempFilePaths
							console.log(this.imageList)
								uni.showLoading({
									 title: '上传中'  
								}); 
							if(this.imageList.length<1){
								if (this.imglist1.length> 8) {
									uni.showToast({
										title:"图片数量超过限制",
										icon:"none"
									})
								    return;
								}
								uni.uploadFile({
								           url: this.newsUrl+'bsiness/tousu1',
								           filePath:this.imageList[0],
								           name: 'img',
								           formData: {
								              
								           },
								           success: (uploadFileRes) =>{
											   console.log(uploadFileRes)
											uni.hideLoading(uploadFileRes);
											var ee=JSON.parse(uploadFileRes.data)
											 console.log(ee)
											this.imglist1.push(ee.img)
											console.log(this.imglist1)
								           }
								       });
							}else{
								var alength=this.imageList.length+this.imglist1.length
								if (alength> 8) {
									uni.showToast({
										title:"图片数量超过限制",
										icon:"none"
									})
								    return;
								}
								for(var i=0;i<this.imageList.length;i++){
									uni.uploadFile({
									           url: this.newsUrl+'bsiness/tousu1',
									           filePath:this.imageList[i],
									           name: 'img',
									           formData: {
									              
									           },
									           success: (uploadFileRes) =>{
												  console.log(uploadFileRes)
												uni.hideLoading(uploadFileRes);
												var ee=JSON.parse(uploadFileRes.data)
												this.imglist1.push(ee.img)
												console.log(this.imglist1)
									           }
									       });
								}
							}
							
							
				        },
				        fail: (err) => {
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
			tssubmit(){
				console.log(this.imglist1)
			}
		}
	}
</script>

<style scoped>
uni-page-body{
	width: 100%;
	height: 100%;
}
.content{
	width: 100%;
	height: 100%;
	display: flex;
	flex-direction: column;
	/* background: #009E94; */
}
.header{
	width: 100%;
	/* height: 60px; */
	/* background: #007AFF; */
	padding-top:var(--status-bar-height);
}
.header_c{
	width: 100%;
	height:40px;
	/* background:#FF0000; */
	display: flex;
	justify-content: space-between;
	align-items: center;
	padding-top: 20px;
	padding: 0px 24px;
	box-sizing: border-box;
}
.header_c_c{
	color:#666666;
}
.header_c_l,.header_c_r{
	width: 11px;
	height: 20px;
	/* background: #00C777; */
}
.header_c_l{
	background: url(../../static/logo.png)no-repeat;
	background-size: 100% 100%;
}
.header_cc{
	width: 100%;
	/* height: 600px; */
	/* background: #007400; */
	padding-bottom: 30px;
}
.photolist{
	width: 100%;
    padding: 10px 0px 0px 0px;
	box-sizing: border-box;
	/* background: pink; */
	display: flex;
	flex-wrap: wrap;
}
.list{
	width: 100%;
	height: 100%;
	background:#f7f7f7;
}
.photo{
	width: 24px;
	height: 22px;
	background:url(https://ss2.bdstatic.com/70cFvnSh_Q1YnxGkpoWK1HF6hhy/it/u=2449171074,3989439360&fm=26&gp=0.jpg) no-repeat;
	background-size: 100% 100%;
}
.submit{
	width: 60px;
	height: 28px;
	background: #77DDCE;
	text-align: center;
	line-height: 28px;
	font-size: 16px;
	color: #ffffff;
}
.header_bo{
	display: flex;
	justify-content: space-between;
	align-items: center;
	/* background: #007AFF; */
	padding: 0px 20px 24px 20px;
	box-sizing: border-box;
}

/* ---上传图片 */
    .cell-pd {
	    padding: 22upx 30upx;
	}
	
	.list-pd {
	    margin-top: 50upx;
	}
	.uni-uploader__file{
		  position: relative;
	}
	.shanchu{ 
		  width: 20px;
		  height: 20px;
		  background: url(https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1594024757181&di=69722be101d7b69ec4da2e162c0f7862&imgtype=0&src=http%3A%2F%2Fbpic.588ku.com%2Felement_origin_min_pic%2F01%2F49%2F34%2F925744624466850.jpg)no-repeat #34495e;
		  background-size: 80% 70%;
		  background-position: center center;
		  border-radius: 5px;
		  position: absolute;
		  padding: 10px;
		  top:0;
		  right: 0;
		  z-index: 5;

	}
	.iconfont {
	    font-family: "iconfont" !important;
	    font-size: 26px;
	    font-style: normal;
	    -webkit-font-smoothing: antialiased;
	    -moz-osx-font-smoothing: grayscale;
	}
/* 上传图片 */
.uni-uploader{
	width: 100%;
}
uni-uploader-body{
	width: 100%;
}
.uni-uploader__files{
	width: 100%;
	/* background: #00C777; */
	display: flex;
	flex-wrap: wrap;
}
.uni-uploader__file{
	width: 32%;
	height: 96px;
	margin:6px 0px 6px 3px;
}
.uni-uploader__img{
	width: 100%;
	height: 100%;
}
</style>
