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
            <select name="dateformat" id="dateformat" v-model="selection">
              <option value="2-digit">numeric format</option>
              <option value="long">long format</option>
            </select>
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
      <span v-if="!fieldsFull">fill required fields</span>

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
        <svg viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
          <title />
          <g>
            <path
              d="M14.41,2H4V22H20V7.59ZM12,18.41l-3.71-3.7,1.42-1.42L11,14.59V10h2v4.59l1.29-1.3,1.42,1.42ZM15,7V5.41L16.59,7Z"
            />
          </g>
        </svg>
        download <strong>png</strong>
      </button>
      <button id="jpeg" class="button save" @click="saveJpeg">
        <svg viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
          <title />
          <g>
            <path
              d="M14.41,2H4V22H20V7.59ZM12,18.41l-3.71-3.7,1.42-1.42L11,14.59V10h2v4.59l1.29-1.3,1.42,1.42ZM15,7V5.41L16.59,7Z"
            />
          </g>
        </svg>
        download <strong>jpeg</strong>
      </button>
      <button id="svg" class="button save" @click="saveSvg">
        <svg viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
          <title />
          <g>
            <path
              d="M14.41,2H4V22H20V7.59ZM12,18.41l-3.71-3.7,1.42-1.42L11,14.59V10h2v4.59l1.29-1.3,1.42,1.42ZM15,7V5.41L16.59,7Z"
            />
          </g>
        </svg>
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
      selection: "2-digit",
      isVisible: false,
      isXvisible: true,
      listItem: {
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
        let formDate = new Intl.DateTimeFormat("en-gb", {
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
  mounted() {},
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
  margin: 0 1em;
}
:is(input, textarea, select):focus {
  outline-color: var(--accent);
}
::-webkit-datetime-edit {
  padding: 0.5em;
}
.ui .button {
  padding: 0.4em 1em;
  background: var(--accent);
  border: none;
  border-radius: 4px;
  cursor: pointer;
  color: rgb(5, 2, 29);
  font-size: 1.2rem;
  font-family: "Open Sans";
}
.ui .disabled {
  background: lightgrey;
  cursor: not-allowed;
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
.buttons span {
  position: absolute;
  top: -30px;
  left: 20px;
  background: #fff;
  padding: 0.1em 0.5em;
  border-radius: 2px;
  opacity: 0;
  font-size: 0.8rem;
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
  top: 15px;
  left: 15px;
  transform: rotate(45deg);
  z-index: -1;
}
.save_btns {
  margin-top: 2em;
  display: flex;
}
.ui .save {
  margin: 0.5em 0.5em 0 0;
  font-size: 0.8rem;
  background: var(--bg-tl);
  display: flex;
  align-items: center;
  gap: 0.2em;
}
strong {
  font-size: 1.2em;
  color: blue;
}
.ui .save:hover {
  font-size: 0.8rem;
  background: var(--accent);
}
.save svg {
  width: 22px;
  margin: auto;
}
.save path{
    fill:var(--bg)

}

</style>