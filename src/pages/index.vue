<template>
	<div class="home_content">
		<div class="module_box">
			<a href="javascript:void(0)" v-for="systemMsg in listMsg" @click="enter(systemMsg.id , systemMsg.url)"><img class="moduleImg" :src="'static/img/'+systemMsg.id+'@2x.png'"/></a>
		</div>
	</div>
</template>

<script>
	export default{
		data(){
			return{
				listMsg:''
			}
		},
		mounted(){
//			if(getCookie("tokenVal")){
//				//调接口
//				//判断token是否有效，有效则可进入系统页面，无效返回登录页
//				this.axios.post(process.env.domain + '/user/checkToken',{token:this.$route.params.token}).then(res => {
//					console.log(res);
//					if(res.data.code == 1004){
//						this.$toast('登录超时，请重新登录',1000)
//						setTimeout(() =>{
//							window.location.href="http://ucenter.beibeiyue.com";
//						},2000)
//					}else if(res.data.code == 1000){
//						setCookie("tokenVal",res.data.result,0.5);
//					}
//				});
//			}else{
//				window.location.href="http://ucenter.beibeiyue.com";
//			}
			
			if(this.$route.params.sign=="noUserName"){
				this.$toast('没有此系统的登录权限',1000)
			}else if(this.$route.params.sign=="stageLocked"){
				this.$toast('本系统内此账号已停用',1000)
			}else if(this.$route.params.sign=="tokenError"){
				this.$toast('登录认证失败 ,请重新登录尝试',1000)
			}else{
				
			}
			
			this.list()
		},
		methods:{
			list(){
				this.axios.post(process.env.domain+'/user/index?token='+this.$route.params.token).then(res => {
					this.listMsg=res.data.result.list;
					console.log(res.data)
				});
			},
			enter(id , url){
//				alert(url)
				if(id==1){
					this.axios.post(process.env.domain +'/user/workOrder?token='+this.$route.params.token).then(res => {
						if(res.data.code==1000){
							window.location.href=url+"?userInfo="+encodeURI(JSON.stringify(res.data.result));
						}else if(res.data.code==1004){
							window.location.href="http://ucenter.beibeiyue.com";
						}else if(res.data.code==1041){
							this.$toast('没有此系统的登录权限',1000)
						}else if(res.data.code==1002){
							this.$toast('当前用户已被锁定，无法登录',1000)
						}else if(res.data.code==1042){
							this.$toast('系统错误-联系管理员',1000)
						}
						
					});
				}else if(id==5){
					this.axios.post(process.env.domain + '/user/checkToken',{token:this.$route.params.token}).then(res => {
						if(res.data.code == 1004){
							this.$toast('登录超时，请重新登录',1000)
							setTimeout(() =>{
								window.location.href="http://ucenter.beibeiyue.com";
							},2000)
						}else if(res.data.code == 1000){
							let msg=eval('(' + res.data.result + ')')
							msg.token =this.$route.params.token;
							msg=JSON.stringify(msg)
							console.log(msg)
							window.location.href=url+"#/login?userInfo="+msg;
						}
					});
				}else{
					window.location.href=url+"?token="+this.$route.params.token;
				}
			}
		}
	}
</script>

<style scoped>
	.home_content{width:100%;height:100%;background:#E9E9E9;}
	.module_box{width:1138px;padding-top:250px;margin:0 auto;}
	.module_box>a{display:inline-block;}
	.moduleImg{width:281px;height:168px;}
	
	@media screen and (max-width: 1280px) {
		.moduleImg{width:263px;height:150px;}
	}
</style>