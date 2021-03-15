<template>
    <div class="home">
        <el-container class="container">
            <!-- 头部 -->
            <el-header class="header">
                <div>
                    <img src="../assets/default-avatar .jpg" alt="" />
                    <span>后台管理系统</span>
                </div>
                <el-button type="info" @click="logout">退出登录</el-button>
            </el-header>
            <!-- 页面主体区域 -->
            <el-container>
                <!-- 侧边栏 -->
                <el-aside :width="isCollapse ? '64px' : '200px'" class="aside">
                    <div class="toggle-button" @click="toggle">|||</div>
                    <!-- 侧边栏菜单区域 -->
                    <el-menu
                        background-color="#545c64"
                        text-color="#fff"
                        active-text-color="#409eff"
                        unique-opened
                        :collapse="isCollapse"
                        :collapse-transition="false"
                        :router="true"
                        :default-active="activePath"
                    >
                        <el-submenu
                            :index="item.id + ''"
                            v-for="item in menulist"
                            :key="item.id"
                        >
                            <template slot="title">
                                <i :class="iconsObj[item.id]"></i>
                                <span>{{ item.authName }}</span>
                            </template>
                            <!-- 二级菜单 -->
                            <el-menu-item
                                :index="'/' + subitem.path"
                                v-for="subitem in item.children"
                                :key="subitem.id"
                                @click="saveNavState('/' + subitem.path)"
                            >
                                <template slot="title">
                                    <!-- 图标 -->
                                    <i class="el-icon-menu"></i>
                                    <!-- 文本 -->
                                    <span>{{ subitem.authName }}</span>
                                </template>
                            </el-menu-item>
                        </el-submenu>
                    </el-menu>
                </el-aside>
                <!-- 右侧内容主体 -->
                <el-main class="main">
                    <!-- 路由占位符 -->
                    <router-view></router-view>
                </el-main>
            </el-container>
        </el-container>
    </div>
</template>

<script>
export default {
    data() {
        return {
            menulist: [],
            iconsObj: {
                125: 'iconfont icon-user',
                103: 'iconfont icon-tijikongjian',
                101: 'iconfont icon-shangpin',
                102: 'iconfont icon-danju',
                145: 'iconfont icon-baobiao',
            },
            //是否折叠
            isCollapse: false,
            //被激活的连接地址
            activePath: '',
        }
    },
    created() {
        this.getMenuList()
        this.activePath = window.sessionStorage.getItem('activePath')
    },
    methods: {
        logout() {
            window.sessionStorage.clear()
            this.$router.push('/login')
        },
        //获取所有的菜单
        async getMenuList() {
            const { data: res } = await this.$http.get('menus')
            if (res.meta.status !== 200)
                return this.$message.error(res.meta.msg)
            this.menulist = res.data
            console.log(res)
        },
        //点击按钮，切换菜单的折叠和展开
        toggle() {
            this.isCollapse = !this.isCollapse
        },
        //保存连接的激活状态
        saveNavState(activePath) {
            window.sessionStorage.setItem('activePath', activePath)
            this.activePath = activePath
        },
    },
}
</script>

<style lang="less">
.home {
    width: 100%;
    height: 100%;
}
.home .container {
    height: 100%;
}
.home .header {
    background: #333;
    display: flex;
    justify-content: space-between;
    align-items: center;
    div {
        display: flex;
        align-items: center;
        img {
            width: 50px;
            height: 50px;
            display: inline-block;
            border-radius: 50%;
        }
        span {
            font-size: 30px;
            color: #fff;
            display: inline-block;
            margin-left: 20px;
        }
    }
    button {
        width: 100px;
        height: 50px;
        font-size: 18px;
        padding: 0;
        margin-right: 50px;
    }
}

.home .aside {
    background: #545c64;
    transition: 1s;
    .el-menu {
        border-right: none;
    }
}
.home .main {
    background: #eee;
}
.home .iconfont {
    margin-right: 10px;
}

.home .toggle-button {
    background: #4a5064;
    line-height: 24px;
    font-size: 10px;
    text-align: center;
    color: #fff;
    letter-spacing: 0.3em;
    cursor: pointer;
}
</style>
