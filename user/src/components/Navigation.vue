<template>
  <div class="menu-preview">
    <el-scrollbar wrap-class="scrollbar-wrapper-horizontal">
      <el-menu class="el-menu-horizontal-demo" :default-active="activeMenu" :unique-opened="true" mode="horizontal" :router="true" @select="handleSelect">
        <div class="left-menu">
          <div class="system-title" @click="goMenu('/index/home')">汽车租赁管理系统</div>
          <div class="menu-items">
            <el-menu-item class="home" index="/index/home" @click.native="goMenu('/index/home')">
              <span class="icon iconfont icon-shouye-zhihui"></span>
              <span>系统首页</span>
            </el-menu-item>
            <el-menu-item class="item" v-for="(menu, index) in menuList" :index="menu.url" :key="index" @click.native="goMenu(menu.url)">
              <i class="icons" :class="iconArr[index]"></i>
              <span>{{menu.name}}</span>
            </el-menu-item>
            <el-menu-item class="service" v-if="Token" @click.native="goChat">
              <span class="icon iconfont icon-touxiang09"></span>
              <span>智能AI</span>
            </el-menu-item>
            <el-menu-item class="item" v-if="Token" index="/index/center" @click.native="goCenter">
              <span class="icon iconfont icon-shouye-zhihui"></span>
              <span>个人中心</span>
            </el-menu-item>
          </div>
        </div>
        <div class="right-menu">
          <template v-if="Token">
            <div class="user-info">
              <el-avatar 
                class="user-avatar" 
                :size="32"
                :src="getAvatarUrl(headportrait)"
              ></el-avatar>
              <span class="welcome-text">欢迎您，</span>
              <span class="username">{{username}}</span>
            </div>
            <el-button class="btn-logout" type="danger" plain @click="logout">
              <i class="el-icon-switch-button"></i>
              <span>退出登录</span>
            </el-button>
          </template>
          <el-button v-else class="btn-login" @click="toLogin">
            <span class="icon iconfont icon-wuliu8"></span>
            <span>登录</span>
          </el-button>
        </div>
      </el-menu>
    </el-scrollbar>
  </div>
</template>

<script>
export default {
  name: 'Navigation',
  props: {
    menuList: {
      type: Array,
      default: () => []
    },
    iconArr: {
      type: Array,
      default: () => []
    },
    baseUrl: {
      type: String,
      default: ''
    },
    Token: {
      type: String,
      default: ''
    },
    username: {
      type: String,
      default: ''
    },
    headportrait: {
      type: String,
      default: ''
    }
  },
  computed: {
    activeMenu() {
      const route = this.$route
      const { meta, path } = route
      if (meta.activeMenu) {
        return meta.activeMenu
      }
      
      // 首先检查完整路径是否匹配菜单项
      if (this.menuList.find(menu => menu.url === path)) {
        return path
      }
      
      // 检查基础路径是否匹配菜单项
      const basePath = '/' + path.split('/').slice(0, 3).join('/')
      
      const menuMatch = this.menuList.find(menu => menu.url === basePath)
      if (menuMatch) {
        return basePath
      }
      
      // 特殊处理个人中心页面跳转的情况
      if (path.includes('/index/') && path !== '/index/home' && path !== '/index/center') {
        // 如果是从个人中心跳转的，将根据实际路径匹配相应菜单项
        for (const menu of this.menuList) {
          // 检查路径中是否包含菜单URL的一部分
          if (menu.url && path.includes(menu.url.replace('/index/', ''))) {
            return menu.url
          }
        }
      }
      
      // 处理详情页和添加页
      if (path.includes('/detail') || path.includes('/add')) {
        const listPath = basePath.replace('Detail', '').replace('Add', '')
        const menuItem = this.menuList.find(menu => menu.url === listPath)
        if (menuItem) {
          return listPath
        }
      }
      
      return path
    }
  },
  methods: {
    handleSelect(keyPath) {
      this.$emit('select', keyPath)
    },
    goMenu(path) {
      // 更新菜单高亮，根据path找到对应的菜单项索引
      let menuIndex = this.menuList.findIndex(menu => menu.url === path);
      if (menuIndex !== -1) {
        this.$emit('select', menuIndex.toString());
      }
      this.$emit('go-menu', path)
    },
    goChat() {
      this.$emit('go-chat')
    },
    goCenter() {
      this.$router.push('/index/center')
      this.$emit('select', '/index/center')
    },
    logout() {
      this.$emit('logout')
    },
    toLogin() {
      this.$emit('to-login')
    },
    getAvatarUrl(headportrait) {
      if (!headportrait) {
        return require('@/assets/avator.png');
      }
      
      // 处理可能包含逗号的情况（多图只取第一个）
      let url = headportrait.split(',')[0];
      
      // 特殊处理错误的头像路径
      if (url.includes('1747475186509.jpg')) {
        return 'http://localhost:8080/springboot0aqexa96/upload/yonghu_touxiang1.jpg';
      }
      
      // 处理已经是完整URL的情况
      if (url.startsWith('http')) {
        return url;
      }
      
      // 处理包含upload路径的情况
      if (url.includes('/upload/')) {
        return 'http://localhost:8080/springboot0aqexa96' + url;
      }
      
      // 默认情况，使用baseUrl拼接
      return this.baseUrl + url;
    }
  }
}
</script>

<style>
.nav-container {
  width: 100%;
  background: #fff;
  box-shadow: 0 2px 12px rgba(0, 0, 0, 0.05);
  position: fixed;
  top: 0;
  left: 0;
  z-index: 100;
}

.nav-content {
  max-width: 1200px;
  margin: 0 auto;
  height: 64px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 20px;
}

.nav-left {
  display: flex;
  align-items: center;
}

.logo {
  height: 40px;
  margin-right: 40px;
}

.nav-menu {
  display: flex;
  align-items: center;
  gap: 32px;
}

.nav-item {
  font-size: 16px;
  color: #333;
  text-decoration: none;
  padding: 8px 0;
  position: relative;
  transition: all 0.3s;
}

.nav-item:hover,
.nav-item.active {
  color: #1B3E90;
}

.nav-item::after {
  content: '';
  position: absolute;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 2px;
  background: #1B3E90;
  transform: scaleX(0);
  transition: transform 0.3s;
}

.nav-item:hover::after,
.nav-item.active::after {
  transform: scaleX(1);
}

.nav-right {
  display: flex;
  align-items: center;
  gap: 24px;
}

.user-avatar {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  cursor: pointer;
  transition: transform 0.3s;
}

.user-avatar:hover {
  transform: scale(1.1);
}

.login-btn,
.register-btn {
  padding: 8px 24px;
  border-radius: 8px;
  font-size: 14px;
  transition: all 0.3s;
}

.login-btn {
  color: #1B3E90;
  border: 1px solid #1B3E90;
  background: transparent;
}

.login-btn:hover {
  background: rgba(27, 62, 144, 0.05);
  transform: translateY(-2px);
}

.register-btn {
  color: #fff;
  background: #1B3E90;
  border: none;
}

.register-btn:hover {
  background: #25489C;
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(27, 62, 144, 0.2);
}

.user-menu {
  position: absolute;
  top: 100%;
  right: 0;
  background: #fff;
  border-radius: 8px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  padding: 8px 0;
  min-width: 160px;
}

.menu-item {
  padding: 12px 20px;
  color: #333;
  font-size: 14px;
  display: flex;
  align-items: center;
  gap: 8px;
  cursor: pointer;
  transition: all 0.3s;
}

.menu-item:hover {
  background: #f5f7fa;
  color: #1B3E90;
}

.menu-item i {
  font-size: 16px;
}

.right-menu {
  display: flex;
  align-items: center;
  padding-right: 20px;
  gap: 20px;
}

.user-info {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 0 8px;
  height: 40px;
  border-radius: 20px;
  background: #f5f7fa;
  transition: all 0.3s;
}

.user-info:hover {
  background: #e8f0fe;
}

.user-avatar {
  border: 2px solid #fff;
  box-shadow: 0 2px 8px rgba(0,0,0,0.1);
}

.welcome-text {
  color: #666;
  font-size: 14px;
}

.username {
  color: #1B3E90;
  font-size: 14px;
  font-weight: 600;
  max-width: 120px;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.btn-logout {
  height: 36px;
  padding: 0 16px;
  border-radius: 18px;
  font-size: 14px;
  border: 1px solid #f56c6c;
  color: #f56c6c;
  transition: all 0.3s;
  display: flex;
  align-items: center;
  gap: 4px;
}

.btn-logout:hover {
  background-color: #f56c6c;
  color: #fff;
}

.btn-logout i {
  font-size: 16px;
}

.btn-login {
  height: 36px;
  padding: 0 20px;
  border-radius: 18px;
  font-size: 14px;
  border: 1px solid #1B3E90;
  color: #1B3E90;
  background: transparent;
  transition: all 0.3s;
  display: flex;
  align-items: center;
  gap: 6px;
}

.btn-login:hover {
  color: #fff;
  background: #1B3E90;
}
</style> 