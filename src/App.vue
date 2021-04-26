<template>
  <Header/>
 <div class="container">
    <List :list="list"/>
    <UI @addItem='addItem' @clearList='clearList'/>

 </div>
  
</template>

<script>

import List from './components/List.vue'
import UI from './components/UI.vue'
import Header from './components/Header.vue'



export default {
  name: 'App',
  components: {
    UI,
   List,
   Header
  },
  data(){
    return{
      list:[]
    }
  },
  methods:{
    addItem(e){
      this.list = e.list
    

    },
     clearList() {
      this.list = [];
      this.listItem = { name: "", date: "", content: "" };
    },
  },
  mounted(){
    
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Manrope:wght@200;400;700;800&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Open+Sans:wght@300;400;700;800&display=swap');

#app {
  font-family: 'Open Sans',Manrope, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  
}
body{
  font-family: 'Open Sans',Manrope, Helvetica, Arial, sans-serif;
  color:var(--text);
  background: var(--bg);
  margin: 0;
  padding: 0;
  min-height: 100vh;
  --bg:#051e35;
  --bg-tl:#f3f3f4;
  --accent:hsl(160, 87%, 52%);
  --accent-h:hsl(160, 100%, 8%);
  --text:rgb(42, 43, 46);
  --text-accent:#478ac2;
 
}
.container {
  display: grid;
 
  grid-template-columns: 1fr 1fr;
  
}

* {
  position: relative;

}
/* TRANSITIONS */

.fade-enter-active,
.fade-leave-active {
  transition: all 0.5s ease;
  transform: translateX(0px);

}
.fade-enter-from,
.fade-leave-to {
  transform: translateX(20px);
  opacity: 0;
}
</style>
