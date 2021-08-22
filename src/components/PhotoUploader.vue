<template>
    <div class="row">
      <div class="col-7">

          <button v-if="!modal" class="button " style="background-color: #2d6072" @click="$refs.uploader.click();test();showSub = false"><strong>Upload Member Picture</strong></button>
      <input type="file" ref="uploader" @change="inputChange();" name="" id="" style="display:none">
      <!-- <div style="width:200px;position:absolute;right:0px;top:70px;" class="bg-dark">
        <button class="btn btn-light"></button>
      </div> -->
      <!--  -->
      <div v-if="modal">
        <div class="container p-5" v-if="percentCompleted != null">
          <hr>
          <div v-if="percentCompleted == 100" style="transition:2s ease-in" class="jumbotron bg-success text-center text-light">
            <div class="mb-3"><i class="fa fa-smile text-light" style="font-size:25pt"></i></div>
            Your avatar uploaded successfully
          </div>
        </div>

        <div v-if="percentCompleted == null" class="  mb-2">
        <div class="col-sm-12 d-flex justify-content-center ">
          <vue-cropper
          style="max-width:100%;height: 100px;max-height: 300px;"
          ref="cropper"
          :view-mode="2"
          :aspect-ratio="1 / 1"
          :src="url"
          preview=".preview"
          alt="Source Image">
          </vue-cropper>
          </div>


          <!-- <div class="d-flex justify-content-center">
            <img  style="width:100px;height:100px" :src="cropImg" alt="">
          </div> -->


        </div>

             <button v-if="percentCompleted == null" class="btn btn-warning fa fa-sync-alt float-left" @click="test();$refs.uploader.click();modal=false"></button>
                <div v-if="percentCompleted == null" class="btn-group float-right">
            <button class="btn btn-danger fa fa-times" @click="stop();modal=false"></button>
            <button class="btn btn-success fa fa-check" @click="uploadProcess();modal=false "></button>
          </div>
          <div class="float-right" v-if ="percentCompleted == 100">
            <button class="btn btn-warning fa fa-check" @click="modal=false;"></button>
          </div>
        </div>



    </div>
        <div style="" class="col-4 d-flex justify-content-end">
              <section class="preview-area box">
        <div v-if="modal==true" class="preview" />
        <div v-if="modal==false" class="cropped-image">
          <img
            v-if="cropImg"
            :src="cropImg"
            alt="Cropped Image"
            style="width:120px;height:120px"
          />
          <div v-else class="crop-placeholder" />
          <button class="btn btn-warning fa fa-sync" @click="modal=true"></button>
        </div>
      </section>
           </div>
    </div>
</template>

<script>
import 'cropperjs/dist/cropper.css';
import VueCropper from 'vue-cropperjs'
import image from '../assets/pic/avatar.jpeg'
// import axios from 'axios'
    export default {
        middleware : 'auths',
        name: "Header",
        components:{
          VueCropper
        },


        methods:{
          test(){
            this.$refs.uploader.value = ''
          },
          stop(){
            this.photoSelect = false
            this.url = this.defImage
            this.cropImg = this.defImage
            this.$refs.uploader.value =''

          },
          inputChange(){
            // let file = this.$refs.uploader.files[0]
            // this.url = URL.createObjectURL(file)
            // this.photoSelect = true
            //
      let file = this.$refs.uploader.files[0]
      if(!file){
        alert('Please select an image file');
        return
      }
      if (file.type.indexOf('image/') === -1) {
        alert('Please select an image file');
        return;
      }
      if (typeof FileReader === 'function') {

        const reader = new FileReader();
        reader.onload = (event) => {
          this.url = event.target.result;
          // rebuild cropperjs with the updated source
          this.$refs.cropper.replace(event.target.result);
        };
        reader.readAsDataURL(file);
      } else {
        alert('Sorry, FileReader API not supported');
      }
      this.photoSelect = true
      this.modal=true
          },

      async uploadProcess() {
      // get image data for post processing, e.g. upload or setting image src
      this.cropImg = this.$refs.cropper.getCroppedCanvas().toDataURL();
      this.loading = true
      // let formData = new FormData();
      const file = this.DataURIToBlob(this.cropImg)
      this.$emit('file' , file)

    //     try{
    //   this.percentCompleted = 1
    //   let res = await axios.post(this.api , formData , {
    //   onUploadProgress: function(progressEvent) {
    //    this.percentCompleted = Math.round((progressEvent.loaded * 100) / progressEvent.total)

    // },
    //     headers: {
    //     'Content-Type': 'multipart/form-data'
    //     },
    //  })
    //     if(res.status == 200){
    //     this.cover = this.cropImg
    //     console.log(this.percentCompleted)
    //     this.percentCompleted = 100
    //     }
    //     }catch (error) {
    //       this.percentCompleted = null
    //             console.log(error)

    //         }

    },
        DataURIToBlob(dataURI) {
        const splitDataURI = dataURI.split(',')
        const byteString = splitDataURI[0].indexOf('base64') >= 0 ? atob(splitDataURI[1]) : decodeURI(splitDataURI[1])
        const mimeString = splitDataURI[0].split(':')[1].split(';')[0]

        const ia = new Uint8Array(byteString.length)
        for (let i = 0; i < byteString.length; i++)
            ia[i] = byteString.charCodeAt(i)

        return new Blob([ia], { type: mimeString })
      }
        },
        data(){
          return{
            modal:false,
            photoSelect : false,
            toggle : false,
            // sideMen : false,
            defImage : image,
            scrolled : false,
            url : image,
            cropImg : image,
            loading : true,
            showSub : false,
            cover : null,
            percentCompleted : null
          }
        }
    }
</script>


<style lang="scss" scoped>
.box{
  border:5px solid #666362;
  -webkit-box-shadow: #FFF 0 -1px 4px, rgb(104, 94, 233) 0 -2px 10px, 0px 50px 21px 14px rgba(196, 196, 196, 0.18); 
box-shadow: 0px 50px 21px 14px #e8e5ec1e;
background: #ECEFCF;
}
.preview-area {
    width: 130px;
    height: 130px;
    border-radius: 100%;
    overflow: hidden;
  
}
.preview-area p {
  font-size: 1.25rem;
  margin: 0;
  margin-bottom: 1rem;
}
.preview-area p:last-of-type {
  margin-top: 1rem;
}
.preview {
  width: 100%;
  height: calc(372px * (9 / 16));
  overflow: hidden;
}
.button.-blue {
  color: white;
  background: #416dea;
}

.button {
  display: flex;
  overflow: hidden;

  margin: 10px;
  margin-left:0;
  padding: 12px 12px;

  cursor: pointer;
  user-select: none;
  transition: all 150ms linear;
  text-align: center;
  white-space: nowrap;
  text-decoration: none !important;
  text-transform: none;
  text-transform: capitalize;

  color: #fff;
  border: 0 none;
  border-radius: 15px;

  font-size: 13px;
  font-weight: 500;
  line-height: 1.3;

  -webkit-appearance: none;
  -moz-appearance:    none;
  appearance:         none;

  justify-content: center;
  align-items: center;
  flex: 0 0 160px;
  width: 15vw;
  margin-top:40px;
  box-shadow: 2px 5px 10px var(--color-smoke);

  &:hover {
    transition: all 150ms linear;

    opacity: .85;
  }

  &:active {
    transition: all 150ms linear;
    opacity: .75;
  }

  &:focus {
    outline: 1px dotted #959595;
    outline-offset: -4px;
  }
}

</style>
