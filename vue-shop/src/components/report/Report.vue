<template>
    <div class="report">
        <!-- 面包屑导航区域 -->
        <el-breadcrumb separator-class="el-icon-arrow-right">
            <el-breadcrumb-item :to="{ path: '/home' }"
                >首页</el-breadcrumb-item
            >
            <el-breadcrumb-item>数据统计</el-breadcrumb-item>
            <el-breadcrumb-item>数据报表</el-breadcrumb-item>
        </el-breadcrumb>
        <!-- 卡片视图 -->
        <el-card>
            <div id="main" style="width: 600px; height: 400px"></div>
        </el-card>
    </div>
</template>
<script>
// 导入echarts
export default {
    data() {
        return {}
    },
    created() {},
    //此时页面上的元素已经渲染完毕了
    mounted() {},
    methods: {},
}
</script>
<style lang="less">
</style>