<template>
  <div class="header">
    <div class="header-button is-left">
      <top-logo></top-logo>
      <!-- breadcrumb按钮和面包屑 -->
      <div class="tags-breadcrumb">
        <i class="icon-navicon tag-collapse" :class="[{ 'tag-collapse_right': isCollapse }]" @click="showCollapse"></i>
        <!-- <top-breadcrumb class="tags-breadcrumb-list"></top-breadcrumb> -->
      </div>
    </div>
    <h1 class="header-title">
      <topMenu></topMenu>
    </h1>
    <div class="header-button is-right">
      <el-tooltip class="item" effect="dark" content="锁屏" placement="bottom">
        <span class="header-item">
          <top-lock></top-lock>
        </span>
      </el-tooltip>
      <el-tooltip class="item" effect="dark" :content="isFullScren?'退出全屏':'全屏'" placement="bottom">
        <span class="header-item">
          <i :class="isFullScren?'icon-tuichuquanping':'icon-quanping'" @click="handleScreen"></i>
        </span>
      </el-tooltip>
      <el-dropdown>
        <span class="el-dropdown-link">
          {{userInfo.username}}
          <i class="el-icon-arrow-down el-icon--right"></i>
        </span>
        <el-dropdown-menu slot="dropdown">
          <el-dropdown-item>
            <router-link to="/">首页</router-link>
          </el-dropdown-item>
          <el-dropdown-item>
            <router-link to="/info/index">个人信息</router-link>
          </el-dropdown-item>
          <el-dropdown-item>
            <a href="https://gitee.com/smallweigit/avue" target="_blank">码云地址</a>
          </el-dropdown-item>
          <el-dropdown-item>
            <a href="https://github.com/nmxiaowei/avue" target="_blank">github</a>
          </el-dropdown-item>
          <el-dropdown-item @click.native="logout" divided>退出系统</el-dropdown-item>
        </el-dropdown-menu>
      </el-dropdown>
      <el-tooltip class="item" effect="dark" content="用户头像" placement="bottom">
        <img class="header-userImg" :src="userInfo.avatar">
      </el-tooltip>
    </div>
  </div>
</template>
<script>
import { mapGetters } from "vuex";
import { fullscreenToggel, listenfullscreen } from "@/util/util";
import topLogo from "./top-logo";
import topLock from "./top-lock";
import topMenu from "./top-menu";
import topBreadcrumb from "./top-breadcrumb";
export default {
  components: { topLock, topLogo, topMenu, topBreadcrumb },
  name: "top",
  data() {
    return {};
  },
  filters: {},
  created() {},
  mounted() {
    listenfullscreen(this.setScreen);
  },
  computed: {
    ...mapGetters([
      "userInfo",
      "isFullScren",
      "tagWel",
      "tagList",
      "isCollapse",
      "tag"
    ])
  },
  methods: {
    handleScreen() {
      fullscreenToggel();
    },
    showCollapse() {
      this.$store.commit("SET_COLLAPSE");
    },
    setScreen() {
      this.$store.commit("SET_FULLSCREN");
    },
    logout() {
      this.$confirm("是否退出系统, 是否继续?", "提示", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        type: "warning"
      }).then(() => {
        this.$store.dispatch("LogOut").then(() => {
          this.$router.push({ path: "/login" });
        });
      });
    }
  }
};
</script>

<style lang="scss" scoped>

</style>

