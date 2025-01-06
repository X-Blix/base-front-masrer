<template>
  <div class="navbar">
    <hamburger :is-active="sidebar.opened" class="hamburger-container" @toggleClick="toggleSideBar" />

    <breadcrumb class="breadcrumb-container" />

    <div class="right-menu">
      <el-dropdown class="avatar-container" trigger="click">
        <div class="avatar-wrapper">
          <!--          <img :src="avatar+'?imageView2/1/w/80/h/80'" class="user-avatar">-->
          <span class="avatar">{{ name.charAt(0) }}</span>
          <!--          用户名称-->
          <span class="name">{{ name }}</span>
          <!--          图标-->
          <i class="el-icon-caret-bottom" />
        </div>
        <el-dropdown-menu slot="dropdown" class="user-dropdown">
          <router-link to="/">
            <el-dropdown-item>
              首页
            </el-dropdown-item>
          </router-link>

<!--          <router-link to="/user/profile">-->
<!--            <el-dropdown-item>个人中心</el-dropdown-item>-->
<!--          </router-link>-->

          <el-dropdown-item style="font-size: 14px; padding: 5px 0">
            <router-link to="/password" style="text-decoration: none">修改密码</router-link>
          </el-dropdown-item>

          <!--          native事件修饰符：注册组件根元素的原生事件-->
          <el-dropdown-item divided @click.native="logout">
            <span style="display:block;">登出</span>
          </el-dropdown-item>
        </el-dropdown-menu>
      </el-dropdown>
    </div>
  </div>
</template>

<script>
import { mapGetters } from 'vuex'
import Breadcrumb from '@/components/Breadcrumb'
import Hamburger from '@/components/Hamburger'
import sysUser from '@/api/system/sysUser'

export default {

  components: {
    Breadcrumb,
    Hamburger
  },
  computed: {
    sysUser() {
      return sysUser
    },
    ...mapGetters([
      // 引入头像和用户名称
      'sidebar',
      'avatar',
      'name'
    ])
  },
  methods: {
    toggleSideBar() {
      this.$store.dispatch('app/toggleSideBar')
    },
    async logout() {
      // 思路：1.需要在单击按钮的时候跳出一个弹出框
      // 进行逻辑判断
      // 如果点击“确定”：调用退出登录的action，返回主页面
      // 如果点击“取消”：提示：用户取消操作，不进行跳转

      // await this.$store.dispatch('user/logout')// 调用退出登录的action
      // this.$router.push(`/login?redirect=${this.$route.fullPath}`)

      // debugger
      this.$confirm('此操作将退出该系统, 是否继续?', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(async(response) => {
        await this.$store.dispatch('user/logout')// 调用退出登录的action
        this.$router.push(`/login?redirect=${this.$route.fullPath}`)
        this.$message({
          type: 'success',
          message: '退出成功!'
        })
      }).catch(() => {
        this.$message.info('取消退出')
      })
    },
    resetPassword() {
      dialogVisible = true
      // // 在点击修改密码方法时弹出框
      // // 根据id查询，数据回显
      // // 弹出框
      // // debugger
      // this.$confirm('此操作将修改密码, 是否继续?', '提示', {
      //   confirmButtonText: '确定',
      //   cancelButtonText: '取消',
      //   type: 'warning'
      // }).then(async(response) => {
      //   await this.$store.dispatch('user/logout')// 调用退出登录的action
      //   this.$router.push(`/login?redirect=${this.$route.fullPath}`)
      //   this.$message({
      //     type: 'success',
      //     message: '修改成功!请重新登录'
      //   })
      // }).catch(() => {
      //   this.$message.info('取消修改')
      // })
    }
  }
}
</script>

<style lang="scss" scoped>
.navbar {
  height: 50px;
  overflow: hidden;
  position: relative;
  background: #fff;
  box-shadow: 0 1px 4px rgba(0,21,41,.08);

  .hamburger-container {
    line-height: 46px;
    height: 100%;
    float: left;
    cursor: pointer;
    transition: background .3s;
    -webkit-tap-highlight-color:transparent;

    &:hover {
      background: rgba(0, 0, 0, .025)
    }
  }

  .breadcrumb-container {
    float: left;
  }

  .right-menu {
    float: right;
    height: 100%;
    line-height: 50px;

    &:focus {
      outline: none;
    }

    .right-menu-item {
      display: inline-block;
      padding: 0 8px;
      height: 100%;
      font-size: 18px;
      color: #5a5e66;
      vertical-align: text-bottom;

      &.hover-effect {
        cursor: pointer;
        transition: background .3s;

        &:hover {
          background: rgba(0, 0, 0, .025)
        }
      }
    }

    .avatar-container {
      margin-right: 30px;
      .avatar-wrapper {
        margin-top: 5px;
        position: relative;
        display: flex;
        align-items: center;

        .name{
          margin-right: 5px;
          margin-left: 5px;
          margin-size:10px ;
        }
        .avatar {
          cursor: pointer;
          width: 30px;
          height: 30px;
          line-height: 30px;
          text-align: center;
          background-color: #04c9be;
          color: #ffffff;
          border-radius: 50%;
          margin-right: 4px;
        }
        .el-icon-caret-bottom{
          margin-right: 10px;
          margin-left: 10px;
          margin-size:16px ;
        }
        //.user-avatar {
        //  cursor: pointer;
        //  width: 40px;
        //  height: 40px;
        //  border-radius: 50%;
        //}

        .el-icon-caret-bottom {
          cursor: pointer;
          position: absolute;
          right: -20px;
          top: 25px;
          font-size: 12px;
        }
      }
    }
  }
}
</style>
