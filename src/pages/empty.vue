<template>
</template>

<script>
	export default{
		data(){
			return{
				
			}
		},
		created(){
			console.log('跳转empty  11111111111111111111111111')
			let logincode=getQuery('code');
			this.axios.post(process.env.domain+'/userLogin/qrLogin?code='+logincode).then(res => {
				console.log(res.data);
				let msg=res.data.result;
				console.log(msg);
				if(res.data.code==1007){
					this.$router.push('/home/personal_msg/'+msg.token+'/'+msg.orgEmail+'/'+msg.mobile+'/'+msg.hiredDate+'/'+'分组'+'/'+msg.persistentCode+'/'+msg.position);
				}else if(res.data.code==1000){
					this.$router.push('/index');
				}else if(res.data.code==1037){
					this.$toast('您不属于鱼乐贝贝的员工',1000)
					setTimeout(() =>{
						this.$router.push('/');
					},2000)
				}
			});
			function getQuery(name) {
				var reg = new RegExp("(^|&)" + name + "=([^&]*)(&|$)", "i");
				var r = window.location.search.substr(1).match(reg);
				if (r != null) return unescape(r[2]); return null;
			}
		}
	}
</script>

<style>
</style>