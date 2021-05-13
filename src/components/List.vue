<template>
  <div class="">
    <div
      class="timeline"
      @mouseenter="isXvisible = true"
      @mouseleave="isXvisible = false"
    >
      <div class="form-timeline">
        <div>
          <label for="darkmode">dark mode</label>
          <label class="switch">
            <input type="checkbox" name="darkmode" v-model="isDarkmode" />
            <span class="slider round"></span>
          </label>
        </div>

        <div>
          <label for="Bullet Color">Bullet</label>
          <input
            type="color"
            id="Bullet Color"
            name="Bullet Color"
            v-model="bullet.color"
          />
        </div>
        <div>
          <label for="bullet_border">Bullet Outline</label>
          <input
            type="color"
            id="bullet_border"
            name="bullet_border"
            v-model="bullet.bordercolor"
          />
        </div>
      </div>
      <div class="wrap">
        <transition-group tag='ul' name="list" mode="out-in">
          <li v-for="(item, index) in sortByDate" :key="item.id" class="list-item">
            <transition name="fade">
              <button
                class="close"
                @click="removeItem(index)"
                v-if="isXvisible"
              ></button>
            </transition>

            <div class="skala"></div>

            <transition appear name="fade">
              <div
                class="bullet"
                :style="{
                  background: bullet.color,
                  'border-color': bullet.bordercolor,
                }"
              ></div
            ></transition>
            <div class="box">
              <transition  appear @enter="enter" 
              >
                <div
                  class="box_inner"
                  :style="[isDarkmode ? userStyle : { 'box-bg': 'white' }]"
                >
                  <p class="date">
                    {{ item.formatedDate }}
                  </p>
                <div class="entry-blok">
                    <p
                      class="name editable"
                      contenteditable="true"
                      @focusout="editName($event, index)"
                    >
                      {{ item.name }}
                    </p>
                      <svg
                         v-if="isXvisible"
                        xmlns="http://www.w3.org/2000/svg"
                       
                        width="16"
                        height="16"
                        viewBox="0 0 24 24"
                        stroke-width="2"
                        stroke="currentColor"
                        fill="none"
                        stroke-linecap="round"
                        stroke-linejoin="round"
                      >
                        <path stroke="none" d="M0 0h24v24H0z" fill="none" />
                        <path
                          d="M9 7h-3a2 2 0 0 0 -2 2v9a2 2 0 0 0 2 2h9a2 2 0 0 0 2 -2v-3"
                        />
                        <path
                          d="M9 15h3l8.5 -8.5a1.5 1.5 0 0 0 -3 -3l-8.5 8.5v3"
                        />
                        <line x1="16" y1="5" x2="19" y2="8" />
                      </svg>
                </div>
                 <div class="entry-blok">
                    <p
                      class="content editable"
                      contenteditable="true"
                      @focusout="editContent($event, index)"
                    >
                     
                      {{ item.content }}
                    </p>
                     <svg v-if="isXvisible"
                        xmlns="http://www.w3.org/2000/svg"
                  
                        width="16"
                        height="16"
                        viewBox="0 0 24 24"
                        stroke-width="2"
                        stroke="currentColor"
                        fill="none"
                        stroke-linecap="round"
                        stroke-linejoin="round"
                      >
                        <path stroke="none" d="M0 0h24v24H0z" fill="none" />
                        <path
                          d="M9 7h-3a2 2 0 0 0 -2 2v9a2 2 0 0 0 2 2h9a2 2 0 0 0 2 -2v-3"
                        />
                        <path
                          d="M9 15h3l8.5 -8.5a1.5 1.5 0 0 0 -3 -3l-8.5 8.5v3"
                        />
                        <line x1="16" y1="5" x2="19" y2="8" />
                      </svg>
                 </div>
                </div>
              </transition>
            </div>
          </li>
        </transition-group>
      </div>
    </div>
  </div>
</template>

<script>
import { gsap } from "gsap";

export default {
  props: ["list","isXvisible"],

  data() {
    return {
      boxbg: "#051e35",
      isVisible: false,
      isDarkmode: false,
      bullet: {
        color: "#003663",
        bordercolor: "#3fb7de",
      },
    };
  },
  methods: {
    enter(el, done) {
      gsap.from(el, {
        opacity: 0,
        duration: 0.9,
        transformOrigin: "0 0",
        rotationY: 90,
        onComplete: done,
        ease: "bounce.out",
      });
    },
    leave(el, done) {
      // console.log("leave");
      gsap.to(el, { duration: 0.8,
       transformOrigin: "0 0",
        opacity: 0,
        onComplete:done
         });
      
    },
    removeItem(i) {
      this.$emit("removeItem", i);
    },
   
    editName(event, index) {
     
      this.$emit("editName", { edit: event.target.innerText, index: index });
    },
    editContent(event, index) {
     
      this.$emit("editContent", { edit: event.target.innerText, index: index });
    },
  },
  computed: {
    userStyle() {
      return { "--box-bg": this.boxbg, "--text": "#fff" };
    },
    sortByDate() {
      return this.list.sort((a, b) => {
        let dateA = new Date(a.date);
        let dateB = new Date(b.date);
        return dateA - dateB;
      });
    },
  },
  mounted() {
    // console.log(this.sortByDate);
    // console.log(window.innerWidth);
    // console.log(this.isXvisible);

    if (window.innerWidth < 1000) {
      this.isXvisible = true;
    }
  },
};
</script>

<style>
.timeline {
  background: var(--bg-tl);
  display: flex;
  flex-direction: column;
  align-items: center;
  overflow-y: auto;
  height: 100vh;
  padding: 9.5em 0;
  box-sizing: border-box;
}
.form-timeline {
  position: absolute;
  top: 6.5em;
  left: 0;
  margin: 0;
  display: flex;
  justify-content: flex-end;
  width: 100%;
}
.form-timeline > div {
  display: flex;
  align-items: center;
  padding: 0 1.2em 0 0;
  gap: 0.25em;
}
.form-timeline input {
  margin: 0 0.2em;
  width: 20px;
  box-sizing: content-box;
}
.form-timeline label {
  font-size: 0.8rem;
}

@media (max-width: 700px) {
  .timeline {
    padding: 1.5em 0;
  }
  .form-timeline {
    top: 1.5em;
  }
}
.wrap {
  display: flex;
  padding: 2em 1em;
  margin-top: 2em;
}
ul {
  list-style: none;
  padding: 0;
  max-width: 50ch;
  font-size: 1rem;
  display: flex;
  flex-direction: column;
  margin: 0;
}
.timeline li {
  width: 250px;
  transition: all 0.5s ease-in-out;
  display: flex;
  position: relative;
  padding: 0 2em;
  perspective-origin: left top;
  perspective: 500px;
}
.close {
  transition: all 0.3s;
  border: none;
  position: absolute;
  right: -40px;
  top: 0px;
  width: 16px;
  height: 16px;
  opacity: 0.3;
  cursor: pointer;
}
.close:hover {
  opacity: 1;
}
.close:before,
.close:after {
  position: absolute;
  top: 0;
  left: 7px;
  content: " ";
  height: 16px;
  width: 2px;
  background-color: rgb(71, 71, 71);
}
.close:before {
  transform: rotate(45deg);
}
.close:after {
  transform: rotate(-45deg);
}
.skala {
  transition: all 0.2s;
  position: absolute;
  top: 24px;
  left: 0;
  width: 2px;
  height: calc(100% - 30px);
  background: #d1d1d1;
  opacity: 1;
  transform: translateY(0);
}
li:last-of-type .skala {
  opacity: 0;
  transform: translateY(30px);
  /* display: none; */
}
.bullet {
  position: absolute;
  top: 0;
  left: -7px;
  width: 10px;
  height: 10px;
  border-radius: 50%;
  border: 4px solid;
}

.box {
  text-align: left;
}

.box_inner {
  transition: background-color 0.3s;
  --box-bg: #fff;
  border-radius: 4px;
  margin: 0 0 0.5em;
  padding: 0.2em 1.5em 0.2em;
  background: var(--box-bg);
  z-index: 0;
  box-shadow: 02px 01px 10px rgba(190, 190, 190, 0.382);
  color: var(--text);
}
.box_inner::before {
  transition: all 0.3s;
  content: "";
  width: 15px;
  height: 15px;
  position: absolute;
  top: 04px;
  left: -04px;
  background: var(--box-bg);
  transform: rotate(45deg);
  z-index: -1;
}

:is(.name, .date, .content):focus {
  border: none;
  outline: none;
  color: var(--btn-active);
  background: rgb(255, 255, 255);
}
p {
  margin: 0 0 0.3em;
  max-width: 25ch;
}
.editable {
  cursor: cell;
}

.name {
  text-transform: uppercase;
  font-size: 1.1rem;
  font-weight: 800;
}.name + svg{
  position: absolute;
  right:-1em;

  top:3px
}
.date {
  padding: 0em 0;
  font-weight: 300;
  letter-spacing: 0.3px;
  font-size: 1.4rem;
  margin: 0 0 0.2em;
  color: var(--text-accent);
}.content + svg{
  position: absolute;
  right:-1em;

  top:3px
}
.content {
  padding: 0em 0.5em 0.5em;
  font-size: 1rem;
  font-weight: 300;
}

/* toggle switch styling */
.switch {
  position: relative;
  display: inline-block;
  width: 45px;
  height: 24px;
}

.switch input {
  opacity: 0;
  width: 0;
  height: 0;
}

.slider {
  position: absolute;
  cursor: pointer;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: #ccc;
  -webkit-transition: 0.4s;
  transition: 0.4s;
}

.slider:before {
  position: absolute;
  content: "";
  height: 17px;
  width: 17px;
  left: 5px;
  bottom: 4px;
  background-color: white;
  -webkit-transition: 0.4s;
  transition: 0.4s;
}

input:checked + .slider {
  background-color: var(--bg);
}

input:checked + .slider:before {
  -webkit-transform: translateX(17px);
  -ms-transform: translateX(17px);
  transform: translateX(17px);
}

/* Rounded sliders */
.slider.round {
  border-radius: 14px;
}

.slider.round:before {
  border-radius: 50%;
}

/* TRANSITIONS */


.list-enter-active, .list-leave-active {
  transition: all 0.5s;
}
.list-enter-from /* .list-leave-active below version 2.1.8 */ {
  opacity: 0;
}
.list-leave-to{
  opacity: 0;
  transform: translateX(-35px);

}

.list-item{
  transition: all 0.5s ease-in-out;
  display: inline-block;
}
.list-move{
  transition:transform  0.3s ease;
}
</style>