<template>
  <div class="wrap">
    <div class="title">
      Animal Classifier
    </div>
    <p class="text">
      The image classifier uses VisionTransformer, a vit, base, 16patch model provided by torchvision, pre-trained in imagenet, without any fine tuning. Because the data is imagenet, it can basically classify animal images, but it also recognizes clothes, objects, and so on.
    </p>
    <!-- <div class='calc'>
      <form @submit.prevent>
        <input class="input" v-model="param.num1" @keyup.enter="submit"> + 
        <input class="input" v-model="param.num2" @keyup.enter="submit"> = 
        {{ num }}
        <button class='button' type="submit" @click="submit">submit</button>
      </form>
    </div> -->
    <div class="container">
      <form class='form' @submit.prevent="upload">
        <UploadImages @changed="handleImages" />
        <div class='form-right'>
          <p class="result">
            this animal is ...<br/><br/>
            ''{{ classify }}''
          </p>
          <button class="button" @click="upload" type="submit">
            <p v-show='!loading'>upload</p>
            <Loading v-show='loading'>
              <vue-loading type='spin' color="#e0e0e0ea" :size="{width: '50px', height: '50px'}"></vue-loading>
            </Loading>
          </button>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
import { VueLoading } from 'vue-loading-template';
export default {
  components:{
    VueLoading,
  },
  data() {
    return {
      num: 0,

      param: {
        num1: 1,
        num2: 6
      },

      img: 'aiueo',

      file: null,
      classify: '***',
      loading: false,
    }
  },
  methods: {
    async submit() {
      console.log({num1: this.param.num1, num2: this.param.num2});
      console.log('submit completed');
      const response = await fetch('https://classify-backend.onrender.com/api/add/', {
        method: 'POST',
        headers: {'Content-Type': 'application/json'},
        body: JSON.stringify(this.param)
      })
      const res = await response.json();
      this.num = res;
    },
    handleImages(files) {
      console.log(files);
      this.file = files[0];
    },
    async upload() {
      this.loading = true;

      let formData = new FormData();
      formData.append('file', this.file)
      formData.append('type', this.file.type)

      const response = await fetch('https://classify-backend.onrender.com/api/predict/', {
        method: 'POST',
        body: formData
      })
      const res = await response.json();
      this.classify = res;

      this.loading = false;
    }
  },
}

</script>

<style>
.wrap {
  margin: 0;
  padding: 10px;
}
.title {
  text-align: center;
  font-size: 2rem;
  color: #e0e0e0ea;
}
.text {
  margin: 0 auto;
  padding: 20px 25%;
}
.calc {
  width: 40%;
  margin: 20px auto;
  padding: 30px;
  display: flex;
  justify-content: center;
  align-items: center;
  border: 2px solid #e0e0e0ea;
  border-radius: 20px;
}
.button {
  display: block;
  margin: 10px auto;
  padding: 30px 80px;
  color: #e0e0e0ea;
  background-color: #444444;
  border: none;
  border-radius: 5px;
  position: absolute;
  bottom: 120px;
  font-size: 1.1rem;
}
.button:hover {
  cursor: pointer;
  transition: all 0.3s ease;
  background-color: #2e2e2e;
}
.input {
  margin: 5px;
  padding: 10px;
  width: 20px;
  text-align: center;
  border-radius: 20%;
  background-color: #444444;
  border: 1px solid #e0e0e0ea;
  color: #e0e0e0ea;
}
.imgsPreview .imageHolder[data-v-69bb59a3] {
  width: 512px;
  height: 512px;
}
.container[data-v-69bb59a3] {
  width: 550px;
  height: 500px;
  background: #2e2e2e;
  padding: 50px;
}
.drop[data-v-69bb59a3] {
  background-color: #2e2e2e;
}
.container {
  margin: 0 auto;
  padding: 30px;
  font-size: 20px;
  width: 50%;
  height: 600px;
  background: #2e2e2e;
  color: #e0e0e0ea;
  justify-content: center;
  display: flex;
}
.form-right {
  width: 200px;
  margin: 10px auto;
  padding: 30px;
  float: left;
}
.result {
  display: flex;
}
.imgsPreview .imageHolder .plus[data-v-69bb59a3] {
  color: #e0e0e0ea;
  background: #2e2e2e;
  border-radius: 50%;
  font-size: 21pt;
  height: 30px;
  width: 30px;
  text-align: center;
  border: 1px dashed;
  line-height: 28px;
  position: absolute;
  right: -42px;
  bottom: 43%;
}
.imgsPreview .imageHolder .delete[data-v-69bb59a3] {
  position: absolute;
  top: 4px;
  right: 4px;
  width: 29px;
  height: 29px;
  color: #fff;
  background: none;
  border-radius: 50%;
}
.clearButton[data-v-69bb59a3] {
  color: #e0e0e0ea;
  position: absolute;
  top: 7px;
  right: 7px;
  background: none;
  border: none;
  cursor: pointer;
}
.beforeUpload .icon[data-v-69bb59a3] {
  width: 150px;
  margin: auto;
  display: block;
  margin-top: 150px;
}
.form {
  display: inline-flex;
}
html, body {
  margin: 0;
  padding: 0;
}
body {
  background-color: #2e2e2e;
  color: #e0e0e0ea;
}
</style>
