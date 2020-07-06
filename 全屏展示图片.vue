<template>
   <view class="content">
	 <scroll-view  v-if="!dist" class="scroll-view_H" scroll-x>
			<image @tap="onclick(index)" class="scroll-view-item_H" v-for="(item,index) of imgList" :key="index" :src="item" mode=""></image>
	  </scroll-view>
	  <swiper v-if="dist" class="swiper" :current="imgListlength" @change="change">
	  	<swiper-item class="swiper-item" v-for="(item,index) of imgList" :key="index">
	  		<image class="swiper-item-img" :src="item" mode=""></image>
			<image @tap="delect" class="delect-img" src="/static/shanchu.png" mode=""></image>
	  	</swiper-item>
	  </swiper>
	  <view v-if="dist" class="bei"></view>
	  <view v-if="dist" class="line">
	  	{{length}} / {{count}}
	  </view>
   </view>
</template>

<script>
export default {
   data() {
      return {
         imgList:[
                             '/static/1.jpg',
                             '/static/2.jpeg',
                             '/static/3.jpg',
                             '/static/4.jpg',
                             '/static/5.jpeg',
             ],
          currentImg:0,  //当前默认选中
		  imgListlength:0,
		  length:1,
		  count:0,
		  dist:false
      }
   },
   onLoad() {
		this.count=this.imgList.length
   },
   methods: {
    change(e){
		console.log(e.detail.current)
		this.length=e.detail.current+1
	},
	onclick(e){
		console.log(e)
		this.dist=true
		this.imgListlength=e
		this.length=e+1
	},
	delect(){
		this.dist=false
	}
   }
};
</script>

<style scoped>
.content{
	width: 100%;
	height: 100%;
	position: relative;
}
.scroll-view_H {
		white-space: nowrap;
		width: 100%;
		border: 1px solid #000000;
}
.scroll-view-item_H {
		display: inline-block;
		width: 100px;
		height:100px;
		margin-left: 11px;
}
.swiper{
	width: 100%;
	height: 90%;
	position: fixed;
	top: 0px;
	left: 0px;
	z-index: 88;
}
.swiper-item-img{
	width: 100%;
	height: 90%;
}
.swiper-item{
	position: relative;
}
.bei{
	width: 100%;
	height: 100%;
	background: rgba(0,0,0,0.5);
	position: fixed;
	top: 0px;
	left: 0px;
}
.line{
	width: 100%;
	height: 30px;
	position: fixed;
	bottom: 40px;
	font-size: 18px;
	display: flex;
	justify-content: center;
}
.delect-img{
	width: 30px;
	height: 30px;
	position: absolute;
	top: 20px;
	right: 20px;
}
</style>
