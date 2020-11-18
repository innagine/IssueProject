<template>
  <div>
    <el-menu
      :default-active="activeIndex"
      class="el-menu-demo"
      mode="horizontal"
      @select="handleSelect"
      background-color="#FFFFFF"
      text-color="#6fb7ff"
      active-text-color="#409EFF"
    >
      <el-menu-item index="1" @click="index">ISSUE首页</el-menu-item>
      <el-menu-item index="2" @click="createIssue">issue创建</el-menu-item>
      <el-menu-item index="3">issue报表</el-menu-item>
      <el-menu-item index="4">账号管理</el-menu-item>
      <el-menu-item index="6" style="float: right">
        <el-button size="small" @click="Register">登陆/注册</el-button>
      </el-menu-item>
      <el-menu-item style="float: right">
        <router-link to="/">
          <div>
            <el-avatar
              src="https://cube.elemecdn.com/0/88/03b0d39583f48206768a7534e55bcpng.png"
            ></el-avatar>
          </div>
        </router-link>
      </el-menu-item>

      <el-menu-item index="5" class="prevent">
        <el-input
          v-model="input"
          placeholder="请输入搜索内容"
          class="changestyle"
        ></el-input>
        <el-button size="small">搜索</el-button>
      </el-menu-item>
    </el-menu>
    <RotationChart v-if="show"></RotationChart>
    <Choose v-if="show" @son="FromSon"></Choose>
    <CreateIssue v-if="showCreateIssue"></CreateIssue>
  </div>
</template>

<script>
// @ is an alias to /src
import RotationChart from "@/components/RotationChart.vue";
import Choose from "@/components/Choose.vue";
import CreateIssue from "@/views/CreateIssue.vue";

export default {
  name: "Home",
  components: {
    RotationChart,
    Choose,
    CreateIssue,
  },
  data() {
    return {
      input: "",
      activeIndex: "",
      show: true,
      showCreateIssue: false,
    };
  },
  methods: {
    Register() {
      this.$router.push({
        path: "/register",
      });
    },
    createIssue() {
      this.showCreateIssue = true;
      this.show = false;
    },
    index() {
      this.showCreateIssue = false;
      this.show = true;
      // this.$router.push({
      //     path: '/createIssue'
      // })
    },
    FromSon(data) {
      if (data == 1) {
        this.createIssue();
        this.activeIndex = "2";
        console.log(this.activeIndex);
      }
    },
  },
};
</script>

<style>

/* 防止搜索框点击之后背景变黑 */
.prevent:hover {
  background-color: #fff !important;
}

/* 调节导航栏字体大小 */
.el-menu-item,
.template {
  font-size: 16px !important;
}

/* 搜索框长度长度变换 */
.changestyle {
  padding-right: 10px;
  transition: all 0.5s;
}

.changestyle:hover {
  width: 400px !important;
}

.el-button {
  background-color: #409eff !important;
  color: white !important;
}
</style>
