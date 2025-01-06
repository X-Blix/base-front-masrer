<template>
  <el-card style="width: 500px">
    <el-form label-width="120px" size="small" :model="form" :rules="rules" ref="pass">

      <el-form-item label="原密码" prop="password">
        <el-input v-model="form.password" autocomplate="off" show-password></el-input>
      </el-form-item>
      <el-form-item label="新密码" prop="newPassword">
        <el-input v-model="form.newPassword" autocomplate="off" show-password></el-input>
      </el-form-item>
      <el-form-item label="新密码" prop="confirmPassword">
        <el-input v-model="form.confirmPassword" autocomplate="off" show-password></el-input>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="save">确认</el-button>
      </el-form-item>
    </el-form>
  </el-card>
</template>

<script>
export default {
  name: "Password",
  data(){
    return{
      form:{},//username,password,newPassword,confirmNewPassword这四个属性
      user:localStorage.getItem("user")?JSON.parse(localStorage.getItem("user")):{},
      rules:{
        password:[
          {required:true,message:'请输入原密码',trigger:'blur'},
          {min:3,message: '长度不少于3位',trigger: 'blur'}
        ],
        newPassword:[
          {required:true,message:'请输入新密码',trigger:'blur'},
          {min:3,message: '长度不少于3位',trigger: 'blur'}
        ],
        confirmPassword:[
          {required:true,message:'请输入密码',trigger:'blur'},
          {min:3,message: '长度不少于3位',trigger: 'blur'}
        ],
      }
    }
  },
  created() {
    this.form.username = this.user.username
  },
  methods:{
    save(){
      this.$refs.pass.validate((valid) =>{
        //如果合法
        if (valid){
          if (this.form.newPassword !== this.form.confirmPassword){
            this.$message.error("2次新输入密码不相同")
            return false
          }
          this.request.post("/user/password",this.form).then(res =>{
            if (res.code ==='200'){
              this.$message.success("修改成功")
              this.$store.commit("logout")
            }else {
              this.$message.error(res.msg)
            }
          })
        }
      })
    },
  }
}
</script>

<style>

</style>
