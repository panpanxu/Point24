<template>
	<view class="content">
		<xwDial ref="delPhone" @returnPhone="returnPhone"></xwDial>
	</view>
</template>

<script>
	import xwDial from '@/components/xw-dial'
	export default {
	    components: {
	        xwDial
	    },
	    data() {
	        return {
	            phone:[],
	            tel:""
	        };
	    },
	    methods: {
	        //回删数据
	        delPhone(){
	            this.$refs.delPhone.delPhone()
	        },
	        //清除全部数据
	        delAll(){
	            this.$refs.delPhone.delAll()
	        },
	        //返回数据
	        returnPhone(data){
	            this.phone = data
	            this.tel = data.join("")
	            console.log(this.phone,this.tel)
	        }
	    },
	}
</script>

<style>
	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}

	.logo {
		height: 200rpx;
		width: 200rpx;
		margin-top: 200rpx;
		margin-left: auto;
		margin-right: auto;
		margin-bottom: 50rpx;
	}

	.text-area {
		display: flex;
		justify-content: center;
	}

	.title {
		font-size: 36rpx;
		color: #8f8f94;
	}
</style>
