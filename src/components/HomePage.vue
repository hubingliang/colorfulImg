<template>
  <div class="homePage">
    <div class="imgBox">
      <img class="img" :src='imgUrl' v-show="imgShow" @click="getImg" id="img" crossorigin="anonymous">
      <div class="loader" v-show="loading">
        <div class="loader-inner">
          <div class="loader-line-wrap">
            <div class="loader-line"></div>
          </div>
          <div class="loader-line-wrap">
            <div class="loader-line"></div>
          </div>
          <div class="loader-line-wrap">
            <div class="loader-line"></div>
          </div>
          <div class="loader-line-wrap">
            <div class="loader-line"></div>
          </div>
          <div class="loader-line-wrap">
            <div class="loader-line"></div>
          </div>
        </div>
      </div>
    </div>
    <div class="color">
      <a href="https://github.com/hubingliang/colorfulImg" target="_blank">
        <svg class="icon" aria-hidden="true">
            <use xlink:href="#icon-GitHub"></use>
        </svg>
      </a>
      <img src="https://i.loli.net/2018/03/27/5aba29b5b97ae.png" alt="" class="author">
      <div class="title">ColorfulImg</div>
      <div class="describe">← Click it to get the main color from the picture</div>
      <div class="Dominant">Dominant Color :</div>
      <div id="color">{{ rgb }}</div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  mounted(){
    this.getImg()
  },
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      imgUrl: '',
      img: null,
      loading: true,
      imgShow: false,
      rgb: null,
    }
  },
  methods:{
    getImg: function(){
      let i = Math.floor(Math.random()*(1000+1))
      let color = document.getElementById('color')
      this.img = document.getElementById('img')
      this.imgUrl = `https://picsum.photos/580/600/?image=${i}`
      //this.imgUrl = `https://picsum.photos/580/600?random`
      this.loading = true
      this.imgShow = false
      this.img.onload = ()=> {
        this.loading = false
        this.imgShow = true
        let rgb = this.colorfulImg(this.img)
        this.rgb = 'rgb('+rgb.r+','+rgb.g+','+rgb.b+')'
        document.body.style.backgroundColor = this.rgb
        color.style.background = this.rgb
        color.style.color = this.rgb
        this.copyColor()
      }
      this.img.onerror = ()=>{
        console.log('error')
        this.getImg()
      }
      
      
    },
    colorfulImg: function(imgEl){
      var blockSize = 5, // only visit every 5 pixels
          defaultRGB = {r:0,g:0,b:0}, // for non-supporting envs
          canvas = document.createElement('canvas'),
          context = canvas.getContext && canvas.getContext('2d'),
          data, width, height,
          i = -4,
          length,
          rgb = {r:0,g:0,b:0},
          count = 0;
      if (!context) {
          return defaultRGB;
      }
      height = canvas.height = imgEl.naturalHeight || imgEl.offsetHeight || imgEl.height;
      width = canvas.width = imgEl.naturalWidth || imgEl.offsetWidth || imgEl.width;
      context.drawImage(imgEl, 0, 0);
      try {
          data = context.getImageData(0, 0, width, height);
      } catch(e) {
          /* security error, img on diff domain */alert('x');
          return defaultRGB;
      }
      console.log
      length = data.data.length;
      while ( (i += blockSize * 4) < length ) {
          ++count;
          rgb.r += data.data[i];
          rgb.g += data.data[i+1];
          rgb.b += data.data[i+2];
      }

      // ~~ used to floor values
      rgb.r = ~~(rgb.r/count);
      rgb.g = ~~(rgb.g/count);
      rgb.b = ~~(rgb.b/count);

      return rgb;
    },
    copyColor: function(){
      let rgb = document.getElementById("color").innerHTML
      var oInput = document.createElement('input');
      oInput.value = rgb;
      document.body.appendChild(oInput);
      oInput.select(); // 选择对象
      document.execCommand("Copy"); // 执行浏览器复制命令
      oInput.className = 'oInput';
      oInput.style.display='none';
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="less">
.homePage{
  height: 600px;
  width: 1020px;
  display: flex;
  .imgBox{
    display: flex;
    justify-content: center;
    align-items: center;
    width: 580px;
    height: 600px;
  }
  .color{
    background: rgb(253, 245, 237);
    flex: 1;
    padding: 60px 30px 60px 40px;
    position: relative;
    .icon{
      position: absolute;
      right: 20px;
      bottom: 20px;
      width: 30px;
      height: 30px;
    }
    .author{
      width: 70px;
      height: 70px;
      border-radius: 50%;
    }
    .title{
      font-size: 40px;
      font-weight: bold;
      margin: 50px 0;
    }
    .describe{
      margin-bottom: 20px;
    }
    .Dominant{

    }
    #color{
      margin-top: 20px;
      width: 100px;
      height: 50px;
      overflow: hidden;
    }
  }
}
</style>
