<template>
  <div class="report">
      <!-- 查询条件 -->
      <div class="report-form">
        <el-form :model="ruleForm" status-icon :rules="rules" ref="ruleForm" label-width="100px" class="demo-ruleForm">
            <el-row>
              <!-- 用户ID查询 -->
              <el-col :span="9">
                <el-form-item label="用户ID" prop="userId" style="width:400px">
                    <el-input v-model="ruleForm.userId" autocomplete="off"></el-input>
                </el-form-item>                
              </el-col>
              <!-- 用户姓名查询 -->
              <el-col :span="8">
                <el-form-item label="用户姓名" prop="userName" style="width:400px">
                    <el-input v-model="ruleForm.userName" autocomplete="off"></el-input>
                </el-form-item>              
              </el-col>
              
              <el-col :span="7">
                <el-form-item>
                    <!-- 查询按钮 -->
                    <el-button type="success" icon="el-icon-search" @click="submitForm('ruleForm')">查询</el-button>
                    <!-- 重置按钮 -->
                    <el-button @click="resetForm('ruleForm')">重置</el-button>
                </el-form-item>
              </el-col>
            </el-row>
        </el-form>
      </div>

      <!-- 查询列表 -->
    <div class="report-table">
      <el-table
        ref="multipleTable"
        :data="userList.slice((currentPage-1)*pagesize,currentPage*pagesize)"
        tooltip-effect="dark"
        style="width: 1315px"
        border
        stripe
        height="520px"
        @selection-change="handleSelectionChange">
        <el-table-column
          type="selection"
          width="55">
        </el-table-column>

        <!-- 序号 -->
        <el-table-column prop="number" label="序号" width="180">
        </el-table-column>

        <!-- 用户ID -->
        <el-table-column prop="userId" label="用户ID" width="180">
        </el-table-column>

        <!-- 用户姓名 -->
        <el-table-column prop="userName" label="用户姓名"  width="180">
        </el-table-column>

        <!-- 创建issue数 -->
        <el-table-column prop="createdNum" label="创建issue数" width="180">
          <template slot-scope="scope">
            <a href="http://www.baidu.com">
              {{scope.row.createdNum}}
            </a>
          </template>        
        </el-table-column>

        <!-- 收到issue数 -->
        <el-table-column prop="receivedNum" label="收到issue数" width="180">
          <template slot-scope="scope">
            <a href="http://www.baidu.com">
              {{scope.row.receivedNum}}
            </a>
          </template>  
        </el-table-column>

        <!-- 修改issue数 -->
        <el-table-column prop="alterNum" label="修改issue数" width="180">
          <template slot-scope="scope">
            <a href="http://www.baidu.com">
              {{scope.row.alterNum}}
            </a>
          </template>  
        </el-table-column>

        <!-- 完成率 -->
        <el-table-column prop="finishRate" label="完成率" width="180">
        </el-table-column>    
      </el-table>
    </div>
      <!-- 分页查询 -->
    <div id="report-pagination" >
        <el-pagination
          @size-change="handleSizeChange"
          @current-change="handleCurrentChange"
          :current-page="currentPage"
          :page-sizes="[5, 10, 20, 40]"
          :page-size="pagesize"
          layout="total, sizes, prev, pager, next, jumper"
          :total="userList.length">
        </el-pagination>        
    </div>
    </div>
</template>


<script>

  export default {
    data() {
      var validateUserId = (rule, value, callback) => {     //用户ID验证方法
        if (value.length>30) {
            callback(new Error("用户ID不能超过30个字符"));
        }
        else{
              callback();
        }
      };
      var validateUserName = (rule, value, callback) => {   //用户姓名验证方法
        if (value.length>30) {
          callback(new Error("用户姓名不能超过30个字符"));
        } else {
          callback();
        }
      };
      return {
        currentPage:1,  //初始页
        pagesize:20,    //每页的长度
        userList:[],    //每页的数据
        multipleSelection: [],
        ruleForm: {
          userId: '',
          userName: ''
        },
        rules: {        //设置表单验证规则
          userId: [
            { validator: validateUserId, trigger: 'blur' }
          ],
          userName: [
            { validator: validateUserName, trigger: 'blur' }
          ]
        }
      };
    },
    methods: {
      //提交方法
      submitForm(formName) {          
        if (this.ruleForm.userId === ''&&this.ruleForm.userName==='') {   //检查表单是否为空
          alert('查询条件不能为空！');
          return;
        } 
        this.$refs[formName].validate((valid) => {    //进行表单验证
          if (valid) {
            alert('submit!');
            this.$https.get('./data/userlist.json').then(res=>{
              this.userList=res.body
            })
          } else {
            console.log('error submit!!');
            return false;
          }
        });
      },
      //重置表单项
      resetForm(formName) {         
        this.$refs[formName].resetFields();
      },
      toggleSelection(rows) {       //设置多选
        if (rows) {
          rows.forEach(row => {
            this.$refs.multipleTable.toggleRowSelection(row);
          });
        } else {
          this.$refs.multipleTable.clearSelection();
        }
      },
      handleSelectionChange(val) {  
        this.multipleSelection = val;
      },
      handleSizeChange:function(size){
        this.pagesize=size;
        console.log(this.currentPage);    //每页下拉显示数据
      },
      handleCurrentChange:function(currentPage){
        this.currentPage=currentPage;
        console.log(this.currentPage);
      },
      handleUserList(){
        this.$http.get('./data/userlist.json').then(res=>{
            this.userList=res.body
        })
      }
    },
    created(){
      this.handleUserList()
    },
  }
</script>

<style lang="scss" scoped>
.report{
    background-image: linear-gradient(to right, #51a4db,#a8e4fa);
    text-align: center;
}
.report-form{
  margin: 60px;
  font-weight: bolder;
  display: inline-block;
}
.report-table{
  margin-bottom: 50px;
  background: #fff;
  display: inline-block;
}
#report-pagination{
  position: fixed;    //固定分页栏到浏览器底部
  bottom: 0;
  left: 0;
  right: 0;
  background-color: #fff;
}

</style>
