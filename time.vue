<template>
	<view>
		<view>
			<input class="inputs" type="text" value="" v-model="input" />
			<button type="default" @tap="select">提交</button>
			<view class="text">
				{{time}}
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				input:'2020-05-08 16:51:07',
				time:''
			}
		},
		onLoad() {
			this.aixo()
		},
		methods: {
			select(){
				this.aixo()
			},
			aixo(){
				var timestampLogin = new Date(this.input).getTime();
				console.log(timestampLogin)
				this.getFormatTime(timestampLogin)
			},
			//    获取当前时间戳
			    getUnix: function () {
			      let date = new Date()
				  let datatime=date.getTime()
			      return datatime;
			    },
			//    获取今天0点0分0秒的时间戳
			    getTodayUnix: function () {
			      let date = new Date()
			      date.setHours(0)
			      date.setMinutes(0)
			      date.setSeconds(0)
			      date.setMilliseconds(0)
			      return date.getTime()
			    },
			//    获取今年1月1日0点0分0秒的时间戳
			    getYearUnix: function () {
			      let date = new Date()
			      date.setMonth(0)
			      date.setDate(1)
			      date.setHours(0)
			      date.setMinutes(0)
			      date.setSeconds(0)
			      date.setMilliseconds(0)
			      return date.getTime()
			    },
			//    获取标准年月
			    getLastDate: function (time) {
			      let date = new Date(time)
			      let month = date.getMonth() + 1 < 10 ? '0' + (date.getMonth() + 1) : date.getMonth() + 1
			      let day = date.getDate() + 1 < 10 ? '0' + (date.getDate() + 1) : date.getDate() + 1
			      return date.getFullYear() + '-' + month + '-' + day
			    },
			//    转换时间
			    getFormatTime: function (timestamp) {
			      let now = this.getUnix()
			      let today = this.getTodayUnix()
			      let year = this.getYearUnix()
			      let timer = (now - timestamp) / 1000
			      let tip = ''
			      if (timer <= 0) {
			        tip = '刚刚'
			      } else if (Math.floor(timer / 60) <= 0) {
			        tip = '刚刚'
			      } else if (Math.floor(timer < 3600)) {
			        tip = Math.floor(timer / 60) + '分钟前'
			      } else if (timer >= 3600 && timer < 86400) {
			        tip = Math.floor(timer / 3600) + '小时前'
			      } else if (timer / 86400 <= 31) {
			        tip = Math.floor(timer / 86400) + '天前'
			      } else if (timer / 2592000 <= 12){
			        tip = Math.floor(timer / 2592000) + '月前'
			      }else if (timer/(60*60*24*365) > 0){
			        tip = Math.floor(timer / (60*60*24*365)) + '年前'
			      }
				   console.log(tip)
				   this.time=tip
			      return tip
			    },
		}
	}
</script>

<style scoped>
.inputs{
	width: 90%;
	height: 40px;
	margin: 0 auto;
	border: 1px solid #000000;
	margin-top: 150px;
}
.text{
	width: 90%;
	height: 40px;
	line-height: 40px;
	font-size: 16px;
	margin:10px auto;
}
</style>
