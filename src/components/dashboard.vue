<template>
  <el-row class="container">
    <!--头部-->
    <el-col :span="24" class="topbar-wrap">
      <div class="topbar-logo topbar-btn">
        <!--<a href="/"><img src="../assets/logo.png" style="width:42px;margin-top: 5px;"></a>-->
      </div>
      <div class="topbar-title topbar-btn">
        <span>管理员用户</span>
      </div>
      <div class="topbar-account topbar-btn">
        <el-dropdown trigger="click">
          <span class="el-dropdown-link userinfo-inner"><i class="iconfont icon-user"></i> {{sysUserName}}  <i class="iconfont icon-down"></i></span>
          <el-dropdown-menu slot="dropdown">
            <el-dropdown-item><router-link to="">个人信息</router-link></el-dropdown-item>
            <el-dropdown-item><router-link :to="''">修改密码</router-link></el-dropdown-item>
            <el-dropdown-item divided @click.native="logout">退出登录</el-dropdown-item>
          </el-dropdown-menu>
        </el-dropdown>
      </div>
    </el-col>
    <!--中间-->
    <el-col :span="24" class="main">
      <!--左侧导航-->
      <aside :class="collapsed?'menu-collapsed':'menu-expanded'">

        <!--展开折叠开关-->
        <div class="menu-toggle" @click.prevent="collapse">
          <i class="iconfont icon-menufold" v-show="!collapsed"></i>
          <i class="iconfont icon-menuunfold" v-show="collapsed"></i>
        </div>

        <!--菜单展开时的显示情况-->

        <el-menu v-show="!collapsed" default-active="0" @open="handleOpen" @close="handleClose" router  ref="menuOpen">
          <template v-for="(item,index) in $router.options.routes" v-if="item.menuShow">
            <el-submenu v-if="!item.leaf" :index="index+''">
              <template slot="title"><i :class="item.iconCls"></i>{{item.name}}</template>
              <el-menu-item v-for="term in item.children" :key="term.path" :index="term.path" v-if="term.menuShow">
                {{term.name}}
              </el-menu-item>
            </el-submenu>
            <el-menu-item v-else-if="item.leaf&&item.children&&item.children.length" :index="item.children[0].path" class="el-submenu__title">
              <i :class="item.iconCls"></i>{{item.children[0].name}}
            </el-menu-item>
          </template>
        </el-menu>
        <!--菜单折叠后的显示情况-->
        <ul v-show="collapsed" class="el-menu collapsed" ref="menuCollapsed">
          <template v-for="(item,index) in $router.options.routes" v-if="item.menuShow">
            <li v-if="!item.leaf" :index="index+''" style="position: relative;">
              <div class="el-submenu__title" style="padding-left: 20px;" @mouseover="showMenu(index,true)" @mouseout="showMenu(index,false)"><i :class="item.iconCls"></i></div>
              <ul class="el-menu submenu" :class="'submenu-hook-'+index" @mouseover="showMenu(index,true)" @mouseout="showMenu(index,false)">
                <li v-for="term in item.children" :key="term.path" v-if="term.menuShow" class="el-menu-item" style="padding-left: 40px;" :class="$route.path==term.path?'is-active':''"
                    @click="$router.push(term.path)">{{term.name}}</li>
              </ul>
            </li>
            <li v-else-if="item.leaf&&item.children&&item.children.length" class="el-menu-item el-submenu__title" :class="$route.path==item.children[0].path?'is-active':''" @click="$router.push(item.children[0].path)">
              <i :class="item.iconCls"></i>
            </li>
          </template>
        </ul>
      </aside>

      <!--右侧内容区-->
      <section class="content-container">
        <div class="grid-content bg-purple-light">
          <el-col :span="24" class="content-wrapper">
            <transition name="fade" mode="out-in">
              <router-view></router-view>
            </transition>
          </el-col>
        </div>
      </section>
    </el-col>
  </el-row>
</template>

<script type="text/ecmascript-6">
  export default {
    data () {
      return {
        sysUserName: '',
        sysUserAvatar: '',
        collapsed: false,
      }
    },
    methods: {
      handleOpen() {
        console.log('handleopen');
      },
      handleClose() {
        console.log('handleclose');
      },
      //折叠导航栏
      collapse: function () {
        this.collapsed = !this.collapsed;
        if(!this.collapsed){
          this.$refs.menuOpen.$el.style.width="180px";
        }
      },
      showMenu(i, status){
        this.$refs.menuCollapsed.getElementsByClassName('submenu-hook-' + i)[0].style.display = status ? 'block' : 'none';
      },
      logout(){
        var _this = this;
        this.$confirm('确认退出吗?', '提示', {
          //type: 'warning'
        }).then(() => {
          sessionStorage.removeItem('access-user');
          _this.$router.push('/login');
        }).catch(() => {

        });
      }
    },
    mounted() {
      /*  api.post(API_URL.OMS_API.USER.PC_LOGIN, loginParams, function (response,) {})*/
    }
  }
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
