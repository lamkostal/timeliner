<template>
  <div class="ui">
    <form>
      <div class="form-left">
        <div class="field">
          <label for="name">Event name</label>
          <div class="control">
            <input type="text" name="name" v-model="listItem.name" />
          </div>
        </div>
        <div class="field">
          <label for="date">Date</label>
          <div class="control">
            <input
              type="date"
              name="date"
              lang="el-GR"
              v-model="listItem.date"
            />
          <div class="field">
              <select name="dateformat" id="dateformat" v-model="selection">
                <option value="2-digit">numeric format</option>
                <option value="long">long format</option>
              </select>
              <select
                name="lang"
                id="lang"
                v-model="lang"
                v-if="selection === 'long'"
              >
                <option value="en-GB">english - UK</option>
                <option value="en-US">english - US</option>
                <option value="gr">greek</option>
                <option value="de">german</option>
                <option value="fr">french</option>
                <option value="es">spanish</option>
                <option value="ita">italian</option>
                <option value="nl">dutch</option>
              </select>
          </div>
          </div>
        </div>

        <div class="field">
          <label for="description">Details</label>
          <div class="control">
            <textarea
              v-model="listItem.content"
              placeholder="add text here"
              rows="6"
              cols="30"
            ></textarea>
          </div>
        </div>
      </div>
    </form>
    <div class="buttons">
      <button
        class="button"
        @mouseover="isVisible = true"
        @click="addListItem(i)"
        :disabled="!fieldsFull"
        :class="{ disabled: !fieldsFull }"
      >
        add item
      </button>
      <span class='additem-pop' v-if="!fieldsFull">fill required fields</span>

      <button
        id="clear"
        class="button"
        @click="clearList"
        :disabled="!list.length"
        :class="{ disabled: !list.length }"
      >
        clear timeline
      </button>
       <button
        id="save"
        class="button"
        @click="saveList"
      >
        save timeline
      </button>
      <span class="tl-pop">timeline saved!</span>

    </div>

     
    
    <div class="save_btns">
      <span>Download as image</span><br>
      <button id="png" class="button save" @click="savePng"    :disabled="!list.length"
        :class="{ disabled: !list.length }">
        <svg viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
          <title />
          <g>
            <path
              d="M14.41,2H4V22H20V7.59ZM12,18.41l-3.71-3.7,1.42-1.42L11,14.59V10h2v4.59l1.29-1.3,1.42,1.42ZM15,7V5.41L16.59,7Z"
            />
          </g>
        </svg>
        <strong>png</strong>
      </button>
      <button id="jpeg" class="button save" @click="saveJpeg"    :disabled="!list.length"
        :class="{ disabled: !list.length }">
        <svg viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
          <title />
          <g>
            <path
              d="M14.41,2H4V22H20V7.59ZM12,18.41l-3.71-3.7,1.42-1.42L11,14.59V10h2v4.59l1.29-1.3,1.42,1.42ZM15,7V5.41L16.59,7Z"
            />
          </g>
        </svg>
       <strong>jpeg</strong>
      </button>
      <button id="svg" class="button save" @click="saveSvg" :disabled="!list.length"
        :class="{ disabled: !list.length }">
        <svg viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
          <title />
          <g>
            <path
              d="M14.41,2H4V22H20V7.59ZM12,18.41l-3.71-3.7,1.42-1.42L11,14.59V10h2v4.59l1.29-1.3,1.42,1.42ZM15,7V5.41L16.59,7Z"
            />
          </g>
        </svg>
         <strong>svg</strong>
      </button>
    </div>
  </div>
</template>

<script>
import domtoimage from "dom-to-image";
import {gsap} from 'gsap';
 
export default {
  data() {
    return {
      lang: "en-GB",
      selection: "2-digit",
      isVisible: false,
      isXvisible: true,
      listItem: {
        id:function(){return Math.random()},
        name: "Simple timeliner",
        date: "",
        formatedDate: "",
        content: "Welcome to my simple timeline tool",
      },
      list: [],
    };
  },
  methods: {
    addListItem(i) {
      if (
        this.listItem.name.length &&
        this.listItem.content.length &&
        this.listItem.date.length
      ) {
        let formDate = new Intl.DateTimeFormat(this.lang, {
          calendar: "gregory",
          year: "numeric",
          month: this.selection,
          day: "2-digit",
        }).format(new Date(this.listItem.date));
        console.log("fd:" + formDate);

        this.listItem.formatedDate = formDate;
        this.list.push(this.listItem);
        this.listItem = { name: "", date: "", content: "", bullet: {} };
        this.$emit("addItem", { list: this.list });
        console.log("emited");
      }
    },
    clearList() {
      this.$emit("clearList");
      this.list = [];
      this.listItem = { name: "", date: "", content: "" };
    },
    saveList(){
      this.$emit('saveList')
      let tlpop = '.tl-pop'
     
      let mytl = gsap.timeline()
      mytl
       .to('.svg_container circle',{
        transformOrigin:'center center',
        fill:'red',
        duration:0.2
      })
      .to('.svg_container circle',{
        scale:1,
        fill:'none',
        stagger:0.1})
      .to(tlpop,{opacity:1,duration:0.2},0)
      .to(tlpop,{y:-10,ease:'bounce.out',duration:0.5},'<')
      .to(tlpop,{delay:0.4,opacity:0,y:0})
     


    },
    savePng() {
      this.isXvisible = false;
      if (this.list.length) {
        let tl = document.querySelector(".wrap");
        domtoimage
          .toPng(tl)
          .then(function (dataUrl) {
            var img = new Image();
            img.src = dataUrl;
            // document.body.appendChild(img);
            var link = document.createElement("a");
            link.download = "mytimeline";

            link.href = dataUrl;
            link.click();
          })
          .catch(function (error) {
            console.error("oops, something went wrong!", error);
          });
      }
    },
    saveJpeg() {
      this.isXvisible = false;
      if (this.list.length) {
        let tl = document.querySelector(".wrap");
        domtoimage
          .toJpeg(tl, { bgcolor: "#fff" })
          .then(function (dataUrl) {
            var img = new Image();
            img.src = dataUrl;
            // document.body.appendChild(img);
            var link = document.createElement("a");
            link.download = "mytimeline";
            link.href = dataUrl;
            link.click();
          })
          .catch(function (error) {
            console.error("oops, something went wrong!", error);
          });
      }
    },
    saveSvg() {
      this.isXvisible = false;
      if (this.list.length) {
        let tl = document.querySelector(".wrap");
        domtoimage
          .toSvg(tl)
          .then(function (dataUrl) {
            console.log(dataUrl);

            var img = new Image();
            img.src = dataUrl;
            // document.body.appendChild(img);
            var link = document.createElement("a");
            link.download = "mytimeline";
            link.href = dataUrl;
            link.click();
          })
          .catch(function (error) {
            console.error("oops, something went wrong!", error);
          });
      }
    },
  },
  computed: {
    fieldsFull() {
      return (
        this.listItem.name.length &&
        this.listItem.content.length &&
        this.listItem.date.length
      );
    },
  },
  mounted() {
    let retrieveddata = localStorage.getItem('mylist') 
   console.log(retrieveddata)
   let storedList = JSON.parse(retrieveddata)

    this.list = storedList   || []
  },
};
</script>

<style>
.ui {
  padding: 9em 3em 01em;
}
form {
  display: flex;
}
.form-right {
  margin: 0 2em;
}
.form-right input {
  width: 20px;
  display: block;
}
.field {
  text-align: left;
  margin: 0.5em 0;
  display: block;
}

.field label {
  display: inline-block;
  margin: 0 0 0.22em 0;
  color: var(--accent);
  font-size: 1rem;
}
.field :is(input, textarea, select) {
  padding: 1em;
  border-radius: 5px;
  border: none;
  /* min-width: 50%; */
  font-size: 0.9rem;
  font-family: "Open Sans";
}
.field select {
  padding: 0.5em;
  margin: 0 0.8em 0 0;
}
:is(input, textarea, select):focus {
  outline-color: var(--accent);
}
::-webkit-datetime-edit {
  padding: 0.5em;
}
.ui .button {
  transition: transform 0.2s;
  padding: 0.4em 1em;
  background: var(--accent);
  border: none;
  border-radius: 4px;
  cursor: pointer;
  color: rgb(5, 2, 29);
  font-size: 1.2rem;
  font-family: "Open Sans";
}
.button:active{
  box-shadow: inset 0 0 15px #fff;
  transform: translateY(-4px);
}
.buttons {
  margin-top: 1.5em;
  width: 100%;
  flex: 1;
  display: flex;
  flex-wrap: wrap;
  justify-content: flex-start;
  align-items: center;
  gap: 20px;
}
.additem-pop{
position: absolute;
  top: -30px;
  left: 20px;
  background: #fff;
  padding: 0.1em 0.5em;
  border-radius: 2px;
  opacity: 0;
  font-size: 0.8rem;
}
.tl-pop{
  position: absolute;
  top: -40px;
  left: 60%;
  background: #fff;
  padding: 0.1em 0.5em;
  border-radius: 2px;
  opacity: 0;
  font-size: 0.9rem;
  
}
.tl-pop::after {
  content: "";
  width: 10px;
  height: 10px;
  background: #fff;
  position: absolute;
  top: 15px;
  left: 55px;
  transform: rotate(45deg);
  z-index: -1;
}
.additem-pop {
  transition: all 0.2s;
  transition-delay: 0.5s;
}
.buttons button:hover + .additem-pop {
  opacity: 1;
  display: block;
}
.additem-pop::after {
  content: "";
  width: 10px;
  height: 10px;
  background: #fff;
  position: absolute;
  top: 15px;
  left: 15px;
  transform: rotate(45deg);
  z-index: -1;
}
.save_btns {
  margin-top: 2em;
  display: flex;
  gap: 0.2em;
}
.save_btns span{
 align-self: flex-end;
  color:var(--accent);
  padding: 0 1em 0em 0;

}
.ui .save {
  margin: 0.5em 0.5em 0 0;
  font-size: 0.8rem;
  background: var(--accent);
  display: flex;
  align-items: center;
  gap: 0.2em;
}
strong {
  font-size: 1.2em;
  color: blue;
}
.save svg {
  width: 22px;
  margin: auto;
}
.save path {
  fill: var(--bg);
}
.ui .disabled {
  background: lightgrey;
  cursor: not-allowed;
}
</style>