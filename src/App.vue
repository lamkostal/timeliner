<template>
  <Header />
  <div class="container">
    <List :list="list" @removeItem="removeItem" 
    @editName="editName($event,index)"
    @editContent="editContent($event,index)"
    />
    <UI @addItem="addItem" @clearList="clearList" @saveList="saveList" />
  </div>
</template>

<script>
import List from "./components/List.vue";
import UI from "./components/UI.vue";
import Header from "./components/Header.vue";

export default {
  name: "App",
  components: {
    UI,
    List,
    Header,
  },
  data() {
    return {
      list: [],
    };
  },
  methods: {
    addItem(e) {
      this.list = e.list;
    },
    removeItem(i) {
      this.list.splice(i, 1);
    },
    editName(e){
      this.list[e.index].name = e.edit
    },
    editContent(e) {
      this.list[e.index].content = e.edit
    },
    clearList() {
      this.list = [];
      this.listItem = { name: "", date: "", content: "" };
    },
    saveList() {
      localStorage.setItem("mylist", JSON.stringify(this.list));
      console.log("listsaved");
    },
  },

  mounted() {
    let retrieveddata = localStorage.getItem("mylist");
    console.log(retrieveddata);
    let storedList = JSON.parse(retrieveddata);

    this.list = storedList || [];
    console.log("list from app:" + this.list);
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Manrope:wght@200;400;700;800&display=swap");
@import url("https://fonts.googleapis.com/css2?family=Open+Sans:wght@300;400;700;800&display=swap");

#app {
  font-family: "Open Sans", Manrope, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  
}
body {
  font-family: "Open Sans", Manrope, Helvetica, Arial, sans-serif;
  color: var(--text);
  background: var(--bg);
  margin: 0;
  padding: 0;
  min-height: 100vh;
  --bg: #051e35;
  --bg-tl: #eeeeee;
  --accent: hsl(160, 77%, 48%);
  --accent-h: hsl(160, 100%, 8%);
  --text: rgb(42, 43, 46);
  --text-accent: #478ac2;
  --btn-active:rgb(104, 50, 255);
}
.container {
  display: grid;
  grid-template-columns: 1fr 1fr;
}
@media (max-width:700px){
  .container {
  display: grid;
  grid-template-columns: 1fr ;
}
.ui{
  padding:5em 1.5em 1.5em;
  grid-row: 1;
}
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
