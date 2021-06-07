<template>
  <el-container class="home-container">
    <!-- 头部区域 -->
    <el-header>
      <div>
        <img src="../assets/电商.png" height="50" width="51px" alt="" />
        <span>电商后台管理系统</span>
      </div>
      <el-button type="info" @click="logout">退出</el-button>
    </el-header>
    <!-- 页面主体区域 -->
    <el-container>
      <!-- 页面侧边栏区域 -->
      <el-aside :width="isCollpase ? '64px':'200px'">
        <div class="toggle-button" @click="toggleCollpase">|||</div>
        <!-- 侧边栏菜单区域 -->
        <el-menu
          :collapse-transition="false"
          :collapse="isCollpase"
          unique-opened
          background-color="#23252c"
          text-color="#fff"
          active-text-color="#409eff"
          :router="true"
          :default-active="activePath"
        >
        <!-- 一级菜单 -->
          <el-submenu :index="item.id + ''" v-for="item in menuList" :key="item.id">
            <!-- 一级菜单的模板区域 -->
            <template slot="title">
              <!-- 图标 -->
              <i :class="iconsObj[item.id]"></i>
              <!-- 文本 -->
              <span>{{item.authName}}</span>     
            </template>
            <!-- 二级菜单 -->
            <el-menu-item :index="'/'+subItem.path" v-for="subItem in item.children" :key="subItem.id" @click="saveNavState('/'+subItem.path)">
              <template slot="title">
                <!-- 图标 -->
                <i class="el-icon-menu"></i>
                <!-- 文本 -->
                <span>{{subItem.authName}}</span>           
              </template>
            </el-menu-item>
          </el-submenu>
        </el-menu>
      </el-aside>
      <!-- 右侧内容主体 -->
      <el-main>
 
        <router-view></router-view>
      </el-main>
    </el-container>
  </el-container>
</template>
<script>
export default {
  data(){
    return{
      //左侧菜单数据
      menuList:[],
      iconsObj:{
        '125':'iconfont icon-user',
        '103':'iconfont icon-tijikongjian',
        '101':'iconfont icon-shangpin',
        '102':'iconfont icon-danju',
        '145':'iconfont icon-baobiao'
      },
      isCollpase:false,
      //被激活的链接地址
      activePath:''
       
      
    }
  },
  created(){
    this.getMenuList();
    this.activePath = window.sessionStorage.getItem('activePath');
    this.activePath = this.activePath;
  },
  methods: {
    logout() {
      window.sessionStorage.clear();
      this.$router.push("/login");
    },
    //获取所有菜单
    async getMenuList(){
      const {data:res} = await this.$http.get('menus');
      if(res.meta.status !==200) return this.$message.error(res.meta.msg)
      this.menuList = res.data
      console.log(res);

    },
    //点击按钮切换菜单的折叠与展开
    toggleCollpase(){
      this.isCollpase = !this.isCollpase;
    },
    //保存链接的激活状态
    saveNavState(activePath){
      window.sessionStorage.setItem('activePath',activePath);
    }
  },
};
</script>
<style>
.el-header {
  background-color: #878d91e1;
  display: flex;
  justify-content: space-between;
  padding-left: 0;
  align-items: center;
  color: #ffff;
  font-size: 17px;
}
.el-header span {
  padding-left: 8px;
}

.el-aside {
  background-color: #23252c;
}
.el-aside >.el-menu{
  border-right: 0;
}
.el-main {
  background-color: rgb(241, 236, 236);
}
.home-container {
  height: 100%;
}
.iconfont{
  margin-right:10px;
}
.toggle-button{
  background-color: #4a5064;
  font-size: 10px;
  line-height: 24px;
  color:#ffff;
  text-align: center;
  letter-spacing: 0.2em;
  cursor:pointer;
}
</style>
