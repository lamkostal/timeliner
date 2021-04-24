<template>
  <div class="ui">
    <form>
      <div class="field">
        <label for="name">Event name</label>
        <div class="control">
          <input type="text" name="name" v-model="listItem.name" />
        </div>
      </div>
      <div class="field">
        <label for="date">date</label>
        <div class="control">
          <input type="date" name="date" lang="el-GR" v-model="listItem.date"/>
        </div>
      </div>

      <div class="field">
        <label for="description">description</label>
        <div class="control">
          <textarea
            v-model="listItem.content"
            placeholder="add multiple lines"
            rows="6"
            cols="40"
          ></textarea>
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
      <span v-if="!fieldsFull">fill all fields</span>

      <button
        id="clear"
        :class="{ disabled: !list.length }"
        class="button"
        @click="clearList"
      >
        clear timeline
      </button>
    </div>
    <div class="save_btns">
      <button id="png" class="button save" @click="savePng">
        download <strong>png</strong>
      </button>
      <button id="jpeg" class="button save" @click="saveJpeg">
        download <strong>jpeg</strong>
      </button>
      <button id="svg" class="button save" @click="saveSvg">
        download <strong>svg</strong>
      </button>
    </div>
  </div>
</template>

<script>
import domtoimage from "dom-to-image";

export default {
  data() {
    return {
      isVisible: false,
      isXvisible: true,
      listItem: {
        name: "Simple timeline",
        date: "",
        content: "Welcome to my simple timeline tool",
      },
      list: [],
    }
  },
  methods:{
     
       addListItem(i) {
      if (
        this.listItem.name.length &&
        this.listItem.content.length &&
        this.listItem.date.length
      ) {
        this.listItem.date = new Intl.DateTimeFormat('el-GR').format(new Date(this.listItem.date))

        this.list.push(this.listItem);
        this.listItem = { name: "", date: "", content: "" };
        this.$emit('addItem',{list:this.list})
        console.log('emited')
      }
    },
     clearList() {
      this.$emit('clearList')
      this.list=[]
      this.listItem = { name: "", date: "", content: "" };
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
   
  },


};
</script>

<style>
    .ui {
  padding: 7em 3em;
}
.field {
  text-align: left;
  width: 80%;
  margin: 1em 0;
}

.field label {
  display: inline-block;
  margin: 0 0 0.22em 0;
  color: var(--accent);
  font-size: 1.1rem;
}
.field :is(input, textarea) {
  padding: 1em;
  border-radius: 5px;
  border: none;
  min-width: 50%;
  font-size: 1rem;
  font-family: 'Open Sans';
}
:is(input, textarea):focus {
  outline-color: var(--accent);
}
::-webkit-datetime-edit {
  padding: 0.5em;
}
.ui .button {
  padding: 0.6em 1em;
  background: var(--accent);
  border: none;
  border-radius: 4px;
  cursor: pointer;
  color: rgb(5, 2, 29);
  font-size: 1.1rem;
  font-family: 'Open Sans';
}
.ui .disabled {
  background: lightgrey;
  cursor: not-allowed;
}
.buttons {
  margin-top: 2em;
  width: 100%;
  flex: 1;
  display: flex;
  flex-wrap: wrap;
  justify-content: flex-start;
  align-items: center;
  gap: 20px;
}
.buttons span {
  position: absolute;
  top: 60px;
  left: 20px;
  background: #fff;
  padding: 0.3em;
  border-radius: 2px;
  opacity: 0;
}
.buttons span {
  transition: all 0.2s;
  transition-delay: 0.5s;
}
.buttons button:hover + span {
  opacity: 1;
  display: block;
}
.buttons span::after {
  content: "";
  width: 10px;
  height: 10px;
  background: #fff;
  position: absolute;
  top: -5px;
  left: 15px;
  transform: rotate(-45deg);
  z-index: -1;
}
.save_btns {
  margin-top: 4em;
  display: flex;
}
.ui .save {
  margin: 0.5em 0.5em 0 0;
  font-size: 0.8rem;
  background: var(--bg-tl);
}
strong {
  font-size: 1.2em;
  color: blue;
}
.ui .save:hover {
  font-size: 0.8rem;
  background: var(--accent);
}
</style>