<template>
  <div>
    <div class="face_upload" @click="uploadImg" >
      拍摄个人正面照片
    </div>
    <button @click="uploadTemImg"> 上传模版</button>
      <!-- 个人照片 -->
      <img :src="imgSrc" mode="aspectFit">
      <!-- 模版照片 -->
      <img :src="temSrc" mode="aspectFit">
      <!-- 最终结果 -->
      <img :src="resultSrc" mode="aspectFit">
    <button @click="geFaceToken"> 颜值评分</button>
    <button @click="mergeImg"> 人脸融合</button>
  </div>
</template>

<script>

export default {
  data () {
    return {
      motto: 'Hello miniprograme',
      userInfo: {
        nickName: 'mpvue',
        avatarUrl: 'http://mpvue.com/assets/logo.png'
      },
      imgSrc:'',
      temSrc:'',
      resultSrc:''
    }
  },

 

  methods: {
    uploadImg(){
      let that=this;
      wx.chooseImage({
        count: 1,
        sizeType: ['original', 'compressed'],
        sourceType: ['album', 'camera'],
        success (res) {
          // tempFilePath可以作为img标签的src属性显示图片
          const tempFilePaths = res.tempFilePaths
          console.log(res.tempFilePaths);
          that.imgSrc=tempFilePaths[0];
        }
      })
    },
    uploadTemImg(){
      let that=this;
      wx.chooseImage({
        count: 1,
        sizeType: ['original', 'compressed'],
        sourceType: ['album', 'camera'],
        success (res) {
          // tempFilePath可以作为img标签的src属性显示图片
          const tempFilePaths = res.tempFilePaths
          console.log(res.tempFilePaths);
          that.temSrc=tempFilePaths[0];
        }
      })
    },
    geFaceToken(){
      this.$http.post('https://api-cn.faceplusplus.com/facepp/v3/detect',{
            api_key:"zfb50yT9zGgeqXEdTPFwB30VVChKmUcI",
            api_secret:"c0hkhiL7e57uH8MCf4U4zByMJb2-r6fX",
            image_base64:wx.getFileSystemManager().readFileSync(this.imgSrc, "base64"),
            return_attributes:'beauty',
      }).then(res=>{
        // this.resultSrc='data:image/jpeg;base64,'+res.data.result;
      })
    },
    beautifyImg(){
      this.$http.post('https://api-cn.faceplusplus.com/facepp/v1/beautify',{
            api_key:"zfb50yT9zGgeqXEdTPFwB30VVChKmUcI",
            api_secret:"c0hkhiL7e57uH8MCf4U4zByMJb2-r6fX",
            image_base64:wx.getFileSystemManager().readFileSync(this.imgSrc, "base64")
      }).then(res=>{
        this.resultSrc='data:image/jpeg;base64,'+res.data.result;
      })
    },
    scoreImg(){
      this.$http.post('https://api-cn.faceplusplus.com/facepp/v1/beautify',{
            api_key:"zfb50yT9zGgeqXEdTPFwB30VVChKmUcI",
            api_secret:"c0hkhiL7e57uH8MCf4U4zByMJb2-r6fX",
            image_base64:wx.getFileSystemManager().readFileSync(that.imgSr, "base64")
      }).then(res=>{
        this.resultSrc='data:image/jpeg;base64,'+res.data.result;
      })
    },
    mergeImg(){
      this.$http.post('https://api-cn.faceplusplus.com/imagepp/v1/mergeface',{
            api_key:"zfb50yT9zGgeqXEdTPFwB30VVChKmUcI",
            api_secret:"c0hkhiL7e57uH8MCf4U4zByMJb2-r6fX",
            template_base64:wx.getFileSystemManager().readFileSync(this.temSrc, "base64"),
            merge_rate:100,
            merge_base64:wx.getFileSystemManager().readFileSync(this.imgSrc, "base64")
      }).then(res=>{
        this.resultSrc='data:image/jpeg;base64,'+res.data.result;
      })
    },
    bindViewTap () {
      const url = '../logs/main'
      if (mpvuePlatform === 'wx') {
        mpvue.switchTab({ url })
      } else {
        mpvue.navigateTo({ url })
      }
    },
    clickHandle (ev) {
      console.log('clickHandle:', ev)
      // throw {message: 'custom test'}
    }
  },

  created () {
    // let app = getApp()
  }
}
</script>

<style scoped>


</style>
