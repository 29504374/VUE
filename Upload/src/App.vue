<template>
  <div id="app">
     <el-tabs v-model="selected">
    <el-tab-pane label="图片上传" name="first">
      <image-upload />
    </el-tab-pane>
    <el-tab-pane label="excel上传" name="second">
      <excel-upload/>
    </el-tab-pane>
    <el-tab-pane label="zip包上传" name="third">
      <zip-upload />
    </el-tab-pane>
    <el-tab-pane label="大文件上传" name="four">更新中...</el-tab-pane>
  </el-tabs>
  </div>
</template>

<script>
import imageUpload from './components/imageComponent'
import excelUpload from './components/excelComponent'
import zipUpload from './components/zipComponent'

export default {
  name: 'App',
  components: {
    imageUpload,
    excelUpload,
    zipUpload
  },
  data()
  {
    return {
      selected:'first'
    }
  }
}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  display: flex;
  justify-content: center;
  .el-tabs
  {
    width: 800px;
  }
}
</style>
