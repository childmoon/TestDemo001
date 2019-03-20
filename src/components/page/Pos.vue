<template>
	<div class="pos">
		<el-row>
			<el-col :span="7" class="pos-order" id="order-list">
				<el-tabs id="pos-toop">
					<el-tab-pane label="点餐">
						<el-table :data="tableData" border style="width:100%">
							<el-table-column prop="goodsName" label="商品名称">
								
							</el-table-column>
							<el-table-column prop="count" label="数量" width="50">
								
							</el-table-column>
							<el-table-column prop="price" label="金额" width="70">
								
							</el-table-column>
							<el-table-column label="操作" width="100" fixed="right">
								<template scope="scope">
									<el-button type="text" size="small" @click="delSingleGoods(scope.row)">
										删除
									</el-button>
									<el-button type="text" size="small" @click="addOrderList(scope.row)">
										增加
									</el-button>
								</template>
							</el-table-column>
						</el-table>
						<div class="totalDiv">
							<small>数量:</small>{{this.totalCount}}&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<small>金额:</small>{{this.totalMoney}}<small>元</small>
						</div>
						<div class="div-btn">
							<el-button type="warning">挂单</el-button>
							<el-button type="danger" @click="delAllGoods()">删除</el-button>
							<el-button type="success" @click="checkout()">结账</el-button>
						</div>
					</el-tab-pane>
					<el-tab-pane label="挂单">
						挂单
					</el-tab-pane>
					<el-tab-pane label="外卖">
						外卖
					</el-tab-pane>
				</el-tabs>
			</el-col>
			<el-col :span="17">
				<div class="often-goods">
					<div class="title">
						常用商品
					</div>
					<div class="often-goods-list">
						<ul>
							<li v-for="goods in oftenGoods" @click="addOrderList(goods)">
								<span>{{goods.goodsName}}</span>
								<span class="o-price">￥{{goods.price}}</span>
							</li>
						</ul>
					</div>
				</div>
				
				<div class="goods-type">
					<el-tabs>
						<el-tab-pane label="汉堡">
							<div>
								<ul class='cookList'>
									<li v-for="goods in type0Goods" @click="addOrderList(goods)">
										<span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
										<span class="foodName">{{goods.goodsName}}</span>
										<span class="foodPrice">￥{{goods.price}}元</span>
									</li>
                                </ul>
							</div>
						</el-tab-pane>
						<el-tab-pane label="小食">
							<div>
								<ul class='cookList'>
									<li v-for="goods in type1Goods" @click="addOrderList(goods)">
										<span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
										<span class="foodName">{{goods.goodsName}}</span>
										<span class="foodPrice">￥{{goods.price}}元</span>
									</li>
								</ul>
							</div>
						</el-tab-pane>
						<el-tab-pane label="饮料">
							<div>
								<ul class='cookList'>
									<li v-for="goods in type2Goods" @click="addOrderList(goods)">
										<span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
										<span class="foodName">{{goods.goodsName}}</span>
										<span class="foodPrice">￥{{goods.price}}元</span>
									</li>
								</ul>
							</div>
						</el-tab-pane>
						<el-tab-pane label="套餐">
							<div>
								<ul class='cookList'>
									<li v-for="goods in type3Goods" @click="addOrderList(goods)">
										<span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
										<span class="foodName">{{goods.goodsName}}</span>
										<span class="foodPrice">￥{{goods.price}}元</span>
									</li>
								</ul>
							</div>
						</el-tab-pane>
					</el-tabs>
				</div>
			</el-col>
		</el-row>
	</div>
</template>

<script>
	import axios from 'axios';
	export default {
		data() {
			return {
			tableData:[],
			oftenGoods:[],
			type0Goods:[],
			type1Goods:[],
			type2Goods:[],
			type3Goods:[],
			totalCount:0,
			totalMoney:0
			};
		},
		created(){
			this.axiosget();
		},
		methods:{
			start(){
				var orderHeight=document.body.clientHeight;
				document.getElementById("order-list").style.height=orderHeight+'px';
			},
			axiosget(){
				axios.get('https://www.easy-mock.com/mock/5b8b30dbf032f03c5e71de7f/kuaican/oftenGoods')
      .then(response=>{
         //console.log(response);
         this.oftenGoods=response.data;
      })
      .catch(error=>{
          console.log(error);
          alert('网络错误，不能访问');
      });
	  
			axios.get('https://www.easy-mock.com/mock/5b8b30dbf032f03c5e71de7f/kuaican/typeGoods')
			.then(response=>{
			//console.log(response);
			this.type0Goods=response.data[0];
			this.type1Goods=response.data[1];
			this.type2Goods=response.data[2];
			this.type3Goods=response.data[3];
			})
			.catch(error=>{
				console.log(error);
				alert('网络错误，不能访问');
			})
			},
			addOrderList(goods){
				//console.log(goods);
				this.totalMoney=0;
				this.totalCount=0;
				
				let isHave = false;
				for(let i=0;i<this.tableData.length;i++){
					//console.log(this.tableData[i].goodsId);
					if(this.tableData[i].goodsId==goods.goodsId){
						isHave = true;
					}
				}
				if(isHave){
					//方法一:过滤器
					let arr = this.tableData.filter(o =>o.goodsId == goods.goodsId);
					console.log(arr);
					arr[0].count++; 
					//方法二:low
					/*for(let k=0;k<this.tableData.length;k++){
						if(this.tableData[k].goodsId==goods.goodsId){
							this.tableData[k].count++;
						}
					}*/
				}else{
					let newGoods={goodsId:goods.goodsId,goodsName:goods.goodsName,price:goods.price,count:1};
					this.tableData.push(newGoods);
				}
				
				this.getAllMoney();
				//结算数据
				/* for(let s=0;s<this.tableData.length;s++){
					this.totalCount+=this.tableData[s].count;
					//this.totalMoney+=goods.price*(this.tableData[s].count?this.tableData[s].count:1);
					this.totalMoney+=this.tableData[s].count*this.tableData[s].price;
				} */
				
			},
			//删除商品
			delSingleGoods(goods){
				//console.log(goods);
				this.tableData=this.tableData.filter(o => o.goodsId !=goods.goodsId);
				this.getAllMoney();
				
			},
			//删除全部商品的代码
			delAllGoods(){
				console.log();
				this.tableData=[];
				this.totalCount=0;
				this.totalMoney=0;
			},
			//模拟结账
			checkout(){
				console.log(this.totalCount);
				if(this.totalCount!=0){
					this.tableData=[];
					this.totalCount=0;
					this.totalMoney=0;
					this.$message({
						message:"结账成功!",
						type:"success"
					});
				}else{
					this.$message.error("当前菜单暂无商品!");
				}
			},
			//汇总数量和金额
			getAllMoney(){
				this.totalCount=0;
				this.totalMoney=0;
				//结算数据代码块
				if(this.tableData){
					for(let s=0;s<this.tableData.length;s++){
						this.totalCount+=this.tableData[s].count;
						//this.totalMoney+=goods.price*(this.tableData[s].count?this.tableData[s].count:1);
						this.totalMoney+=this.tableData[s].count*this.tableData[s].price;
					}
				}
			}
		},
		mounted(){
			this.start();
		}
	}
</script>

<style>
	#pos-toop{
		margin-left: 20px;
	}
	.pos-order{
		background-color: #F9FAFC;
		border-right:1px solid #C0CCDA;
		height: 100%;
	}
	.totalDiv{
		padding:10px;
		font-weight:bold;
	}
	.div-btn{
		margin-top: 10px;
	}
	.title{
		heifht:20px;
		border-bottom: 1px solid #D3dce6;
		background-color: #F9FAFC;
		padding:10px;
		text-align: left;
	}
	.often-goods-list ul li{
		list-style: none;
		float: left;
		border:1px solid #E5E9F2;
		padding:10px;
		margin: 10px;
		background-color: #FFF;
	}
	.o-price{
		color:#58B7FF;
	}
	.goods-type{
		clear: both;
		margin-left: 10px;
	}
	.cookList li{
       list-style: none;
       width:23%;
       border:1px solid #E5E9F2;
       height: auot;
       overflow: hidden;
       background-color:#fff;
       padding: 2px;
       float:left;
       margin: 2px;
   }
   .cookList li span{
        display: block;
        float:left;
   }
   .foodImg{
       width: 40%;
	   height: 52px;
   }
   .foodName{
       font-size: 16px;
       padding-left: 10px;
       color:brown;
   }
   .foodPrice{
       font-size: 16px;
       padding-left: 10px;
       padding-top:10px;
   }
   
</style>
