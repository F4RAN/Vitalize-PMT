<template>
<div style="position:relative;height:100%">

    <div v-if="state == 'view'">
        <div class="d-flex justify-content-between mt-3">
            <div style="width:40%" class="">
            <div class="input-group mb-3">
                <input type="text" class="form-control" placeholder="Search member name or skill ..." aria-label="Member name or " aria-describedby="basic-addon2">
                <div class="input-group-append">
            <div class="btn-outline-secondary btn search-btn" ><i class="fa fa-search"></i></div>
            </div>
        </div>
        </div>
        <div class="">
             <button @click="state = 'add'" class="btn btn-outline-light" type="button"><i class="fa fa-user-plus"></i></button>
        </div>

    </div>
        <div class="members-part mt-3">
        <h3>Members </h3><small>Double click if you want change member status</small>
            <div class="row mt-2">
                <div class="col-md-3 members-cont">
                    <div class="members-pic" @click="onClick($event)">
                        <span class="member-statue "
                         :class="activeMember ? 'statue-busy' : ''"></span>
                        <img src="../assets/pic/pic1.jpeg">
                    </div>
                    <h5 class="mt-2">Anna Lee</h5>
                    <h6>FrontEnd Developer</h6>

                </div>
                <div class="col-md-3 members-cont">
                    <div class="members-pic">
                        <span class="member-statue"></span>
                        <img src="../assets/pic/pic2.jpeg">
                    </div>
                    <h5 class="mt-2">Jack New</h5>
                    <h6>SEO</h6>

                </div>
                <div class="col-md-3 members-cont">
                    <div class="members-pic">
                        <span class="member-statue"></span>
                        <img src="../assets/pic/pic3.jpeg">
                    </div>
                    <h5 class="mt-2">Eliza Bae</h5>
                    <h6>Data Analyst</h6>

                </div>
                <div class="col-md-3 members-cont">
                    <div class="members-pic">
                        <span class="member-statue "></span>
                        <img src="../assets/pic/pic4.jpeg">
                    </div>
                    <h5 class="mt-2">Jeff Ben</h5>
                    <h6>Backend Developer</h6>
                </div>

            </div>
    </div>
</div>
<div v-if="state == 'add'">    
    <h3><i @click="state= 'view'" class="btn btn-info fa fa-angle-left" style="margin-right:20px"></i> Add new member
        <span style="float:right" class="float-right"> 
            <div class="btn-group">
                <button  @click="active = true" :class="active ? 'btn btn-sm text-light btn-statue-active' : 'btn btn-sm btn-statue-active-not'" class="btn btn-sm statue-btns">Active</button>
                <button @click="active = false" :class="!active ? 'btn btn-sm text-light btn-statue-busy' : 'btn btn-sm text-light btn-statue-busy-not'" class="btn btn-sm statue-btns">Busy</button>
            </div>
    </span>
    </h3>

    <hr>
    <div class="row">
            <div class="form-group col-5">
        <div class="">
            <label>Name :</label>
            <div class="input-group">
                <input class="form-control form-input-design"
                v-model="name"
                 type="text" placeholder="Please enter name or nickname">

            </div>
                <small  v-if="nameWarning == true " class="fa fa-info-circle" style="color: #ff6b6b"> Just alphabets !</small>
            <hr class="mt-5 mb-2">
        </div>
        
    </div>
  <div class="col-5"><label for="">Title : </label>
    <div class="input-group ">
        <input type="text" class="form-control" placeholder="e.g. : Senior Developer , Data Analyst , ... ">
    </div></div>
    </div>
                <div class="form-group mt-1" style="width:100%">
                <label class="mb-0" for="">Photo : <i> <small> ( optional )</small></i></label>
                <photo-uploader   @file="addFile($event)"></photo-uploader>
            </div>
    <hr style="margin-top:100px" class=" mb-5">
    <div class="row">
            <div class="col-5">
        <label for="">Skills : </label>
    <div class="input-group">
        <input type="text" class="form-control"><button class="btn far fa-layer-plus" style="background-color:#3388a5;color: whitesmoke"></button>
    </div>
    <div class="mt-3 container" style="padding: 0;">
        <span class="p-2 m-1" style="cursor:pointer;display:inline-block;border:1px solid #c1c1c1;border-radius: 5px">Hello <i class="fa fa-trash" style="color:#ff5252;"></i></span>
        <span class="p-2 m-1" style="cursor:pointer;display:inline-block;border:1px solid #c1c1c1;border-radius: 5px">Hello <i class="fa fa-trash" style="color:#ff5252;"></i></span>
        <span class="p-2 m-1" style="cursor:pointer;display:inline-block;border:1px solid #c1c1c1;border-radius: 5px">Hello <i class="fa fa-trash" style="color:#ff5252;"></i></span>
        <span class="p-2 m-1" style="cursor:pointer;display:inline-block;border:1px solid #c1c1c1;border-radius: 5px">Hello <i class="fa fa-trash" style="color:#ff5252;"></i></span>
        <span class="p-2 m-1" style="cursor:pointer;display:inline-block;border:1px solid #c1c1c1;border-radius: 5px">And 5 more ... </span>
    </div>
    </div>
            <div class="col-5"><label for="">More Information : </label>
    <div class="input-group ">
        <textarea type="text" class="form-control"></textarea>
    </div></div>

</div>
<button class="btn btn-sm btn-block w-100 submit-btndes">Sumbit</button>
</div>
</div>
</template>
<script>
import photoUploader from './PhotoUploader.vue'
export default {
    watch:{
        name(){
            let regex = new RegExp((/^[A-Z]+$/i || /^[A-Za-z]+$/))
            this.nameWarning = regex.test(this.name) ?  false :  true
            console.log(this.nameWarning)
        }
    },
    data(){
        return{
            name : '',
            nameWarning : null,
            active : true,
            state : 'view',
            profile : '',
            activeMember : false,
                    result: [],
        delay: 700,
        clicks: 0,
        timer: null
        }
    },
    methods:{
onClick(event) {
          this.clicks++;
          if (this.clicks === 1) {
            this.timer = setTimeout( () => {
              this.result.push(event.type);
             console.log('sngl')

              this.clicks = 0
            }, this.delay);
          } else {
             clearTimeout(this.timer);  
             this.result.push('dblclick');
             this.activeMember = !this.activeMember
             console.log('dbl')
             this.clicks = 0;
          }         
        } ,
     addFile(e){
        const reader = new FileReader();
        reader.readAsDataURL(e);
        reader.onloadend = (event) => {
            this.profile = event.target.result
        }
        },
    },
    components:{
     photoUploader   
    }
}
</script>

<style scoped>
.search-btn:hover {
color: #6c757d !important;
text-decoration: none;
background: transparent!important;
border-color: #6c757d!important;
cursor: default!important;
}
.members-cont{height: 200px;padding: 10px;transition: cubic-bezier(0.165, 0.84, 0.44, 1) 0.3s;border-radius: 10px }
.form-control:focus{
    box-shadow :none;
    background-color: transparent;
    border-color: white;
}

.members-pic {
    width: 120px;
    height: 120px;
    background-color: aqua;
    margin: 0 auto;
    border-radius: 100%;

    border: solid 2px #dedede;
    position: relative;
}
.members-pic img{width: 100%;height: 100%;border-radius: 100%;}
.members-cont h5{
    text-align: center;
    font-weight: bold;
}
.members-cont h6{
    text-align: center;
    font-weight: normal;
    color: #babbd5;
    font-size: 14px;
}
.members-cont:hover{background-color: #242439;
    cursor: pointer;}
.member-statue{
    width: 18px;height: 18px;background-color: #2178f8;;
    position: absolute;top: 5px;right: 12px;border-radius: 100%;border: solid 2px white;
}
    .statue-busy{background-color: #ff3030
    }

label{
    color: whitesmoke;
}
.form-control{
    background-color: transparent;border: 1px solid #737496;font-size: 14px;color: whitesmoke!important;
}

.submit-btndes{
    position:absolute;bottom:0;background-image: linear-gradient(120deg,#74c7c3,#5d2fdb);
    border: none;color: white;height: 40px;transition: .3s;font-weight: bold
}
.submit-btndes:hover {transform: scale(1.01);color: white;
}
    textarea{resize: none;height: 100px;font-size: 12px;}
    .btn-statue-active{color: #0c63e4;background-color: #2178f8;}
    .btn-statue-active-not{color: white;border: solid 1px #737496;}
    .btn-statue-busy{color: red;background-color: #ff3030
    }
    .btn-statue-busy-not{color: white;border: solid 1px #737496;}
    .statue-btns:hover{color: white;}
    .statue-btns:focus{outline: none;box-shadow: none}
</style>