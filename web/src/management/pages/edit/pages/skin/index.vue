<template>
  <div class="skin-content">
    <div class="navbar-tab">
      <el-radio-group v-model="activeRouter">
        <el-radio-button
          v-for="btnItem in btnList"
          :key="btnItem.router"
          :label="btnItem.text"
          :value="btnItem.router"
        />
      </el-radio-group>
    </div>
    <router-view></router-view>
  </div>
</template>
<script>
export default {
  name: 'skinPage',
  props: {},
  data() {
    return {
      activeRouter: this.$route.name,
      btnList: [
        {
          text: '内容页',
          router: 'QuestionSkinSetting',
          key: 'skinsettings',
          next: true
        },
        {
          text: '结果页',
          router: 'QuestionEditResultConfig',
          key: 'status'
        }
      ]
    }
  },
  watch: {
    activeRouter: {
      handler(val) {
        this.$router.push({ name: val })
      }
    }
  }
}
</script>
<style lang="scss" scoped>
.skin-content {
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
  width: 100%;
  height: 100%;

  .navbar-tab {
    position: absolute;
    top: 10px;
    cursor: pointer;
    z-index: 9999;
    :deep(.el-radio-button__original-radio + .el-radio-button__inner) {
      font-size: 12px;
      height: 28px;
    }
    :deep(.el-radio-button__original-radio:checked + .el-radio-button__inner) {
      color: $primary-color;
      background-color: #fff;
    }
  }
}
</style>
