<template>
  <div >
    <div class="toolbar" >
      <div style="position:absolute;width:90%;height:50px;;-webkit-app-region: drag;"></div>
      <img src="./assets/logo.png" class="nav-logo" alt="">
     <div style="z-index:999"> <i @click="minimizeApplication()" class="fa fa-window-minimize minimize-btn"></i></div>
     <div style="z-index:999"> <i @click="toggle ? maximizeApplication() : unmaximizeApplication()" class="fa fa-window-maximize maximize-btn" ></i></div>
     <div style="z-index:999"> <i @click="quitApplication()" class="fa fa-times close-btn"></i></div>
  </div>

   <div  class="bg-transparent" style="width:100%">
   <div  style="width:100%;" class=" bg-transparent">
     <div style="height:100vh;display:inline-block" class="col-3 sidemenu">
       <div>
         <ul style="font-size:14pt;padding:0">
           <li class="side-list" @click="state = 'dashboard'" :class="state == 'dashboard' ? 'side-list selected' : 'side-list'"><i class="fa fa-tachometer" style="margin-right:5px"></i> Dashboard</li>
           <li class="side-list" @click="state = 'project'" :class="state == 'project' ? 'side-list selected' : 'side-list'"><i class="fas fa-project-diagram" style="margin-right:5px"></i> Projects</li>
           <li class="side-list" @click="state = 'member'" :class="state == 'member' ? 'side-list selected' : 'side-list'"><i class="fa fa-users" style="margin-right:5px"></i> Members</li>
           <li class="side-list" @click="state = 'payment'" :class="state == 'payment' ? 'side-list selected' : 'side-list'"><i class="fa fa-money-check-alt" style="margin-right:5px"></i> Payments</li>
           <li class="side-list" @click="state = 'employer'" :class="state == 'employer' ? 'side-list selected' : 'side-list'"><i class="fa fa-user-tie" style="margin-right:9px"></i> Employers</li>
         </ul>
       </div>
     </div>
     <div style="height:100vh;float:right" class="col-9  main-content">
       <app-members v-if="state == 'member' "></app-members>
     </div>
   </div>
   </div>
  </div>
</template>

<script>
import {app} from '@electron/remote'
import {remote} from 'electron'
import Members from './components/Members.vue'
export default {
  name: 'App',
  data(){
    return{
      state : 'dashboard',
      toggle:!remote.getCurrentWindow().isMaximized()
    }
  },
  components: {
    appMembers : Members
  },
  methods:{
    unmaximizeApplication(){
      console.log('unmaximize')
      remote.getCurrentWindow().unmaximize();
      this.toggle = !this.toggle
    },
    maximizeApplication(){
      console.log('maximize')
      remote.getCurrentWindow().maximize();
      this.toggle = !this.toggle
    },
    minimizeApplication(){
      remote.getCurrentWindow().minimize();
    },
    quitApplication(){
      console.log('closed')
      app.quit()
    }
  }
}
</script>

<style>
*{
  padding:0;
  -webkit-touch-callout: none; /* iOS Safari */
    -webkit-user-select: none; /* Safari */
     -khtml-user-select: none; /* Konqueror HTML */
       -moz-user-select: none; /* Old versions of Firefox */
        -ms-user-select: none; /* Internet Explorer/Edge */
            user-select: none; /* Non-prefixed version, currently
                                  supported by Chrome, Edge, Opera and Firefox */
}
body,html{
  background-color: #222338;;

  color: aliceblue!important;
}
li{
  color:#737496
}
.side-list{
  padding:15px 0px 15px 30px;cursor:pointer;list-style-type: none;
  margin-top:10px;
  transition: cubic-bezier(0.165, 0.84, 0.44, 1) 0.1s;
  border-radius: 0 10px 10px 0;
}
.side-list:hover{
  background: #1f1f31;
}
.selected{
  transition: ease-in-out 0.6s;
  color: #1b1c31;
  background:  whitesmoke;;
}
.selected:hover{
  color: #1b1c31;
  background:  whitesmoke;;
}
.sidemenu{
  padding:10px;
  padding-left:0px;
  padding-top: 50px;
  background-color: #222338;
  border-right: 1px solid rgb(55, 55, 66); 
}
.main-content{
  padding:20px;
  padding-top: 50px;
  background: #1b1c31; 
}
.close-btn{
  color:rgb(68, 68, 68);position:absolute;right:15px;top:8px;font-size:20pt;
  font-weight:10;
  cursor:pointer;
  
}
.maximize-btn{
  color:rgb(68, 68, 68);position:absolute;right:48px;top:8px;font-size:20pt;
  font-weight:5;
  cursor:pointer;
  
}
.minimize-btn{
  color:rgb(68, 68, 68);position:absolute;right:90px;top:8px;font-size:20pt;
  font-weight:5;
  cursor:pointer;
  
}
.minimize-btn:hover{
  color: rgb(255, 255, 255)!important;
}
.maximize-btn:hover{
  color: rgb(255, 255, 255)!important;
}
.close-btn:hover{
  color: rgb(255, 255, 255)!important;
}
.toolbar{
  position:fixed;
  top:0;
  z-index: 1;
  left:0;
  background-color: #161727; 
  width:100%;
  height:45px;
  border-bottom:1px solid rgb(55, 55, 66); ;
  
}
.nav-logo{
  width:30px;
  position: absolute;
  left:5px;
  top:5px
}
</style>
