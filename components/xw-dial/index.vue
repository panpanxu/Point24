<template>
	<view>
		<view class="dialed">
			<view class="dialed_list">
				{{ phone[0] }}
			</view>
			<view class="dialed_list">
				{{ phone[1] }}
			</view>
			<view class="dialed_list">
				{{ phone[2] }}
			</view>
			<view class="dialed_list">
				{{ phone[3] }}
			</view>
		</view>
		
		<view class="can">
			{{ isCan }}
		</view>
		
		<view class="dial">
			<view class="dial_list" @click="addPhone(item)" v-for="(item,i) in list" :key="i">
				{{item.name}}
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				list: [{
						id: "A",
						name: "A",
					},
					{
						id: "2",
						name: "2",
					},
					{
						id: "3",
						name: "3",
					},
					{
						id: "4",
						name: "4",
					},
					{
						id: "5",
						name: "5",
					},
					{
						id: "6",
						name: "6",
					},
					{
						id: "7",
						name: "7",
					},
					{
						id: "8",
						name: "8",
					},
					{
						id: "9",
						name: "9",
					},
					{
						id: "×",
						name: "×",
					},
					{
						id: "10",
						name: "10",
					},
					{
						id: "√",
						name: "√",
					}
				],

				phone: [],
				
				isActive: false,
				
				isCan: "...",
			}
		},
		onLoad() {

		},
		methods: {
			//点击数字键
			addPhone(item) {
				if (isNaN(parseInt(item.name))) {
					if(item.name=='A'){
						this.phone.push(1)
					}
					if(item.name=='×'){
						this.phone = []
						this.isCan = '...'
					}
					if(item.name=='√'){
						this.isCan = '计算中'
						var result = equalTo24(this.phone[0],this.phone[1],this.phone[2],this.phone[3])
						console.log(result)
						if (`It's not possible!` == result){	
							this.isCan = '换牌吧'
						}else{
							this.isCan = '别急再想想'
						}
					}
				} else {
					if(this.phone.length==4){
						this.phone = []
						this.isCan = '...'
					}
					this.phone.push(parseInt(item.name))
				}
				// this.returnPhone()
				// console.log(this.phone)
			},
			//删除键
			delPhone() {
				this.phone.pop()
				// console.log(this.phone)
				this.returnPhone()
			},
			//清除全部
			delAll() {
				this.phone = []
				this.returnPhone()
			},
			//返回数据
			returnPhone() {
				this.$emit("returnPhone", this.phone)
			}
		},
	}
	
	function  equalTo24(a,b,c,d){
	    var ys = ['+','-','*','/'];
	    for(var i=0;i<4;i++){
	        var arrAll = [a,b,c,d];
	        var outArr = [];
	        outArr[0] = arrAll[i];
	        arrAll.splice(i,1);
	        for(let k=0;k<4;k++){       
	            for(let j=0;j<3;j++){
	                var outArr1 = [...outArr];
	                outArr1.push(ys[k]);
	                var arrAll1 = [...arrAll];
	                outArr1.push(arrAll1[j]);
	                arrAll1.splice(j,1);                
	                for(let k=0;k<4;k++){                   
	                    for(let x=0;x<2;x++){
	                        var outArr2 = [...outArr1];
	                        outArr2.push(ys[k]);            
	                        var arrAll2 = [...arrAll1];
	                        outArr2.push(arrAll2[x]);
	                        arrAll2.splice(x,1);                        
	                        for(let k=0;k<4;k++){                   
	                            var outArr3 = [...outArr2];
	                            outArr3.push(ys[k]);
	                            outArr3.push(arrAll2[0]);
	                            var outEx = insertBrackets(outArr3)
	                            if(outEx) return outEx
	                        }
	                    }
	                }
	            }
	        }
	    }
	    return `It's not possible!`
	}
	Array.prototype.insert = function (index, item) {
	      this.splice(index, 0, item);
	      return this
	}
	function insertBrackets(arr){
	    let expressionObj={
	        ex1:[...arr].insert(3,')').insert(0,'('),
	        ex2:[...arr].insert(5,')').insert(0,'('),
	        ex3:[...arr].insert(5,')').insert(2,'('),
	        ex4:[...arr].insert(7,')').insert(2,'('),
	        ex5:[...arr].insert(7,')').insert(4,'('),
	        ex6:[...arr].insert(7,')').insert(4,'(').insert(3,')').insert(0,'('),
	        ex7:[...arr].insert(5,')').insert(3,')').insert(0,'(').insert(0,'('),
	        ex8:[...arr].insert(7,')').insert(5,')').insert(2,'(').insert(2,'('),
	        ex9:[...arr].insert(5,')').insert(5,')').insert(2,'(').insert(0,'('),
	        ex10:[...arr].insert(7,')').insert(7,')').insert(4,'(').insert(2,'(')   
	    }
	    for(let value of Object.values(expressionObj)){     
	        var outStr = value.join().replace(/,/g,'');
	        if(eval(outStr)==24) return outStr
	    }
	    
	}
</script>

<style lang="scss" scoped>
	.can{
		display: grid;
		grid-template-columns: 400rpx;
		grid-template-rows: 139rpx;
		grid-gap: 29rpx 59rpx;
		margin: 0 auto;
		font-size: 56rpx;
		color: #333333;
		line-height: 100rpx;
		text-align: center;
		overflow: hidden;
		font-weight: bold;
	}
	
	.dialed {
		display: grid;
		grid-template-columns: 100rpx 100rpx 100rpx 100rpx;
		grid-template-rows: 139rpx;
		grid-gap: 29rpx 59rpx;
		margin-top: 30rpx;
		background: #FFFFFF;

		.dialed_list {
			width: 100rpx;
			height: 100rpx;
			font-size: 56rpx;
			color: #333333;
			line-height: 100rpx;
			text-align: center;
			background: #F6F6F6;
			border-radius: 50%;
			overflow: hidden;
			font-weight: bold;
		}
	}

	.dial {
		display: grid;
		grid-template-columns: 139rpx 139rpx 139rpx;
		grid-template-rows: 139rpx 139rpx 139rpx 139rpx;
		grid-gap: 29rpx 89rpx;
		margin: 0 auto;
		background: #FFFFFF;

		.dial_list {
			width: 139rpx;
			height: 139rpx;
			font-size: 56rpx;
			color: #333333;
			line-height: 139rpx;
			text-align: center;
			background: #F6F6F6;
			border-radius: 50%;
			overflow: hidden;
			font-weight: bold;
		}

		.dial_list:active {
			color: #2C7FFB;
			background: #2C7FFB;
			background: rgba(44, 127, 251, 0.1);
		}
	}
</style>
