<!--<template>-->
<!--  <div class="app-container">-->
<!--    <div class="search-div">-->
<!--      <el-form label-width="70px" size="small">-->
<!--        <el-row>-->
<!--          <el-col :span="8">-->
<!--            <el-form-item label="关 键 字">-->
<!--              <el-input v-model="searchObj.keyword" style="width: 95%" placeholder="用户名/姓名/手机号码" />-->
<!--            </el-form-item>-->
<!--          </el-col>-->
<!--          <el-col :span="8">-->
<!--            <el-form-item label="操作时间">-->
<!--              <el-date-picker-->
<!--                v-model="createTimes"-->
<!--                type="datetimerange"-->
<!--                range-separator="至"-->
<!--                start-placeholder="开始时间"-->
<!--                end-placeholder="结束时间"-->
<!--                value-format="yyyy-MM-dd HH:mm:ss"-->
<!--                style="margin-right: 10px;width: 100%;"-->
<!--              />-->
<!--            </el-form-item>-->
<!--          </el-col>-->
<!--        </el-row>-->
<!--        <el-row style="display:flex">-->
<!--          <el-button type="primary" icon="el-icon-search" size="mini" @click="fetchData()">搜索</el-button>-->
<!--          <el-button icon="el-icon-refresh" size="mini" @click="resetData">重置</el-button>-->
<!--        </el-row>-->
<!--      </el-form>-->
<!--    </div>-->

<!--    &lt;!&ndash; 工具条 &ndash;&gt;-->
<!--    <div class="tools-div">-->
<!--      <el-button type="success" icon="el-icon-plus" size="mini" @click="add">添 加</el-button>-->
<!--    </div>-->

<!--    &lt;!&ndash; 列表 &ndash;&gt;-->
<!--    <el-table-->
<!--      v-loading="listLoading"-->
<!--      :data="list"-->
<!--      stripe-->
<!--      border-->
<!--      style="width: 100%;margin-top: 10px;"-->
<!--    >-->

<!--      <el-table-column-->
<!--        label="序号"-->
<!--        width="70"-->
<!--        align="center"-->
<!--      >-->
<!--        <template slot-scope="scope">-->
<!--          {{ (page - 1) * limit + scope.$index + 1 }}-->
<!--        </template>-->
<!--      </el-table-column>-->

<!--      <el-table-column prop="username" label="用户名" width="180" />-->
<!--      <el-table-column prop="name" label="姓名" width="110" />-->
<!--      <el-table-column prop="phone" label="手机" />-->
<!--      <el-table-column label="状态" width="80">-->
<!--        <template slot-scope="scope">-->
<!--          &lt;!&ndash;          如果出现报错可以无视掉&ndash;&gt;-->
<!--          <el-switch-->
<!--            :value="scope.row.status===1"-->
<!--            @change="switchStatus(scope.row)"-->
<!--          />-->
<!--        </template>-->
<!--      </el-table-column>-->
<!--      <el-table-column prop="createTime" label="创建时间" />-->

<!--      <el-table-column label="操作" align="center" fixed="right">-->
<!--        <template slot-scope="scope">-->
<!--          <el-button type="primary" icon="el-icon-edit" size="mini" title="修改" @click="edit(scope.row.id)" />-->
<!--          <el-button type="danger" icon="el-icon-delete" size="mini" title="删除" @click="removeDataById(scope.row.id)" />-->
<!--          <el-button type="warning" icon="el-icon-baseball" size="mini" title="分配角色" @click="showAssignRole(scope.row)" />-->
<!--        </template>-->
<!--      </el-table-column>-->
<!--    </el-table>-->

<!--    &lt;!&ndash; 分页组件 &ndash;&gt;-->
<!--    <el-pagination-->
<!--      :current-page="page"-->
<!--      :total="total"-->
<!--      :page-size="limit"-->
<!--      style="padding: 30px 0; text-align: center;"-->
<!--      layout="total, prev, pager, next, jumper"-->
<!--      @current-change="fetchData"-->
<!--    />-->

<!--    <el-dialog title="分配角色" :visible.sync="dialogRoleVisible">-->
<!--      <el-form label-width="80px">-->
<!--        <el-form-item label="用户名">-->
<!--          <el-input disabled :value="sysUser.username" />-->
<!--        </el-form-item>-->
<!--        <el-form-item label="角色列表">-->
<!--          <el-checkbox v-model="checkAll" :indeterminate="isIndeterminate" @change="handleCheckAllChange">全选-->
<!--          </el-checkbox>-->
<!--          <div style="margin: 15px 0;" />-->
<!--          <el-checkbox-group v-model="userRoleIds" @change="handleCheckedChange">-->
<!--            <el-checkbox v-for="role in allRoles" :key="role.id" :label="role.id">{{ role.roleName }}</el-checkbox>-->
<!--          </el-checkbox-group>-->
<!--        </el-form-item>-->
<!--      </el-form>-->
<!--      <div slot="footer">-->
<!--        <el-button type="primary" size="small" @click="assignRole">保存</el-button>-->
<!--        <el-button size="small" @click="dialogRoleVisible = false">取消</el-button>-->
<!--      </div>-->
<!--    </el-dialog>-->
<!--    <el-dialog title="添加/修改" :visible.sync="dialogVisible" width="40%">-->
<!--      <el-form ref="dataForm" :model="sysUser" label-width="100px" size="small" style="padding-right: 40px;">-->
<!--        <el-form-item label="用户名" prop="username">-->
<!--          <el-input v-model="sysUser.username" />-->
<!--        </el-form-item>-->
<!--        <el-form-item v-if="!sysUser.id" label="密码" prop="password">-->
<!--          <el-input v-model="sysUser.password" type="password" />-->
<!--        </el-form-item>-->
<!--        <el-form-item label="姓名" prop="name">-->
<!--          <el-input v-model="sysUser.name" />-->
<!--        </el-form-item>-->
<!--        <el-form-item label="手机" prop="phone">-->
<!--          <el-input v-model="sysUser.phone" />-->
<!--        </el-form-item>-->
<!--      </el-form>-->
<!--      <span slot="footer" class="dialog-footer">-->
<!--        <el-button size="small" icon="el-icon-refresh-right" @click="dialogVisible = false">取 消</el-button>-->
<!--        <el-button type="primary" icon="el-icon-check" size="small" @click="saveOrUpdate()">确 定</el-button>-->
<!--      </span>-->
<!--    </el-dialog>-->
<!--  </div>-->
<!--</template>-->

<!--<script>-->
<!--import api from '@/api/system/sysUser'-->
<!--import roleApi from '@/api/system/sysRole'-->

<!--const defaultForm = {-->
<!--  id: '',-->
<!--  username: '',-->
<!--  password: '',-->
<!--  name: '',-->
<!--  phone: '',-->
<!--  status: 1-->
<!--}-->
<!--export default {-->
<!--  data() {-->
<!--    return {-->
<!--      listLoading: false, // 数据是否正在加载-->
<!--      list: null, // banner列表-->
<!--      total: 0, // 数据库中的总记录数-->
<!--      page: 1, // 默认页码-->
<!--      limit: 10, // 每页记录数-->
<!--      searchObj: {}, // 查询表单对象-->

<!--      createTimes: [],-->

<!--      dialogVisible: false,-->
<!--      sysUser: {},-->
<!--      saveBtnDisabled: false,-->

<!--      dialogRoleVisible: false,-->
<!--      allRoles: [], // 所有角色列表-->
<!--      userRoleIds: [], // 用户的角色ID的列表-->
<!--      isIndeterminate: false, // 是否是不确定的-->
<!--      checkAll: false // 是否全选-->

<!--    }-->
<!--  },-->
<!--  // 生命周期函数：内存准备完毕，页面尚未渲染-->
<!--  created() {-->
<!--    console.log('list created......')-->
<!--    // 调用列表方法-->
<!--    this.fetchData()-->
<!--  },-->

<!--  // // 生命周期函数：内存准备完毕，页面渲染成功-->
<!--  mounted() {-->
<!--    console.log('list mounted......')-->
<!--  },-->
<!--  methods: {-->
<!--    // 切换用户状态-->
<!--    switchStatus(row) {-->
<!--      row.status = row.status === 1 ? 0 : 1-->
<!--      api.updateStatus(row.id, row.status).then(response => {-->
<!--        if (response.code) {-->
<!--          this.$message.success(response.message || '操作成功')-->
<!--          this.fetchData()-->
<!--        }-->
<!--      })-->
<!--    },-->
<!--    // 列表方法-->
<!--    fetchData(page = 1) {-->
<!--      this.page = page-->
<!--      if (this.createTimes && this.createTimes.length === 2) {-->
<!--        this.searchObj.createTimeBegin = this.createTimes[0]-->
<!--        this.searchObj.createTimeEnd = this.createTimes[1]-->
<!--      }-->
<!--      api.getPageList(this.page, this.limit, this.searchObj)-->
<!--        .then(response => {-->
<!--          this.list = response.data.records-->
<!--          this.total = response.data.total-->
<!--        })-->
<!--    },-->
<!--    // 根据id查询，数据回显-->
<!--    edit(id) {-->
<!--      // 弹出框-->
<!--      this.dialogVisible = true-->
<!--      // 调用接口查询-->
<!--      api.getUserId(id)-->
<!--        .then(response => {-->
<!--          this.sysUser = response.data-->
<!--        })-->
<!--    },-->
<!--    // 添加或者修改方法-->
<!--    saveOrUpdate() {-->
<!--      if (!this.sysUser.id) {-->
<!--        this.save()-->
<!--      } else {-->
<!--        this.update()-->
<!--      }-->
<!--    },-->
<!--    // 添加弹框的方法-->
<!--    add() {-->
<!--      this.dialogVisible = true-->
<!--      this.sysUser = {}-->
<!--    },-->
<!--    // 添加-->
<!--    save() {-->
<!--      api.save(this.sysUser).then(response => {-->
<!--        this.$message.success('操作成功') // 提示信息-->
<!--        this.dialogVisible = false// 关闭弹框-->
<!--        this.fetchData(this.page)// 刷新-->
<!--      })-->
<!--    },-->

<!--    // 更新-->
<!--    update() {-->
<!--      api.update(this.sysUser).then(response => {-->
<!--        this.$message.success('操作成功')-->
<!--        this.dialogVisible = false-->
<!--        this.fetchData(this.page)-->
<!--      })-->
<!--    },-->
<!--    // 重置查询表单-->
<!--    resetData() {-->
<!--      console.log('重置查询表单')-->
<!--      this.searchObj = {}-->
<!--      this.createTimes = []-->
<!--      this.fetchData()-->
<!--    },-->

<!--    // 删除（根据id）-->
<!--    removeDataById(id) {-->
<!--      // debugger-->
<!--      this.$confirm('此操作将永久删除该记录, 是否继续?', '提示', {-->
<!--        confirmButtonText: '确定',-->
<!--        cancelButtonText: '取消',-->
<!--        type: 'warning'-->
<!--      }).then((response) => {-->
<!--        // 调用方法删除-->
<!--        api.removeById(id)-->
<!--          .then(response => {-->
<!--            // 提示-->
<!--            this.$message({-->
<!--              type: 'success',-->
<!--              message: '删除成功!'-->
<!--            })-->
<!--            // 刷新-->
<!--            this.fetchData()-->
<!--          })-->
<!--      }).catch(() => {-->
<!--        this.$message.info('取消删除')-->
<!--      })-->
<!--    },-->
<!--    // 展示分配角色-->
<!--    showAssignRole(row) {-->
<!--      this.sysUser = row-->
<!--      this.dialogRoleVisible = true-->
<!--      roleApi.getRolesByUserId(row.id).then(response => {-->
<!--        this.allRoles = response.data.allRoles-->
<!--        console.log(this.userRoleIds)-->
<!--        this.userRoleIds = response.data.userRoleIds-->
<!--        console.log(this.userRoleIds)-->
<!--        this.checkAll = this.userRoleIds.length === this.allRoles.length-->
<!--        this.isIndeterminate = this.userRoleIds.length > 0 && this.userRoleIds.length < this.allRoles.length-->
<!--      })-->
<!--    },-->
<!--    /*-->
<!--        全选勾选状态发生改变的监听-->
<!--        */-->
<!--    handleCheckAllChange(value) { // value 当前勾选状态true/false-->
<!--      // 如果当前全选, userRoleIds就是所有角色id的数组, 否则是空数组-->
<!--      this.userRoleIds = value ? this.allRoles.map(item => item.id) : []-->
<!--      // 如果当前不是全选也不全不选时, 指定为false-->
<!--      this.isIndeterminate = false-->
<!--    },-->
<!--    /*-->
<!--        角色列表选中项发生改变的监听-->
<!--        */-->
<!--    handleCheckedChange(value) {-->
<!--      const { userRoleIds, allRoles } = this-->
<!--      this.checkAll = userRoleIds.length === allRoles.length && allRoles.length > 0-->
<!--      this.isIndeterminate = userRoleIds.length > 0 && userRoleIds.length < allRoles.length-->
<!--    },-->
<!--    // 分配角色-->
<!--    assignRole() {-->
<!--      const assginRoleVo = {-->
<!--        userId: this.sysUser.id,-->
<!--        roleIdList: this.userRoleIds-->
<!--      }-->
<!--      roleApi.assignRoles(assginRoleVo).then(response => {-->
<!--        this.$message.success(response.message || '分配角色成功')-->
<!--        this.dialogRoleVisible = false-->
<!--        this.fetchData(this.page)-->
<!--      })-->
<!--    }-->
<!--  }-->
<!--}-->
<!--</script>-->

<!--<style scoped lang="scss">-->

<!--</style>-->
