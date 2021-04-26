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
            <input type="checkbox" name="darkmode" v-model='isDarkmode'/>
            <span class="slider round"></span>
          </label>
        </div>

        <div>
          <label for="Bullet Color">Bullet Color</label>
          <input
            type="color"
            id="Bullet Color"
            name="Bullet Color"
            v-model="bullet.color"
          />
        </div>

        <div>
          <label for="bullet_border">Outer Color</label>
          <input
            type="color"
            id="bullet_border"
            name="bullet_border"
            v-model="bullet.bordercolor"
          />
        </div>
      </div>
      <div class="wrap">
        <ul>
          <li v-for="(item, index) in sortByDate" :key="item.name">
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
              <transition
                appear
                @enter="enter"
                @before-leave="beforeLeave"
                @leave="leave"
              >
                <div class="box_inner" :style="[isDarkmode? userStyle:{'box-bg':'white'}]">
                  <p class="date" contentEditable="true">
                    {{ item.formatedDate }}
                  </p>
                  <p class="name" contentEditable="true">{{ item.name }}</p>
                  <p class="content" contentEditable="true">
                    {{ item.content }}
                  </p>
                </div>
              </transition>
            </div>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
import { gsap } from "gsap";

export default {
  props: ["list"],
 
  data() {
    return {
      boxbg:'#051e35',
      isVisible: false,
      isXvisible: true,
      isDarkmode:false,
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

    beforeLeave(el, done) {
      console.log("befleave");

      gsap.set(el, { transformOrigin: "0 0", rotationY: 0, onComplete: done });
    },
    leave(el, done) {
      console.log("leave");
      gsap.to(el, { duration: 0.8, transformOrigin: "0 0", opacity: 0 });
      done();
    },

    removeItem(i) {
      this.list.splice(i, 1);
    },
  },
  computed: {
     userStyle(){
      return {'--box-bg':this.boxbg,'--text':'#fff' }
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
    console.log(this.sortByDate);
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
.wrap {
  display: flex;
  padding: 1em 0.8em 0.5em;
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
li {
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
  position: absolute;
    top: 24px;
    left: 0;
    width: 2px;
    height: calc(100% - 30px);
    background: #d1d1d1;
}
li:last-of-type .skala {
  display: none;
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
  --box-bg:#fff;
  border-radius: 4px;
  margin: 0 0 0.5em;
  padding: 0.2em 1em 0.2em;
  background:var(--box-bg);
  z-index: 0;
  box-shadow: 02px 01px 10px rgba(190, 190, 190, 0.382);
  color: var(--text);
}
.box_inner::before {
  content: "";
  width: 15px;
  height: 15px;
  position: absolute;
  top: 04px;
  left: -04px;
  background:var(--box-bg);
  transform: rotate(45deg);
  z-index: -1;
}

:is(.name, .date, .content):focus {
  border: none;
  outline: none;
  color: var(--text-accent);
  background: rgb(247, 247, 247);
}
p {
  margin: 0 0 0.3em;
  max-width: 25ch;
}
.box > * {
  /* background: rgb(214, 214, 214); */
  cursor: text;
}
.name {
  text-transform: uppercase;
  font-size: 1rem;
  font-weight: 700;
  
}
.date {
  padding: 0.1em 0;
  font-weight: 300;
  letter-spacing: 0.5px;
  font-size: 1.25rem;
  margin: 0 0 0.2em;
  color: var(--text-accent);
  
}
.content {
  padding: 0em 0.5em 0.5em;
  font-size: 0.95rem;
  font-weight: 300;
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
}
.form-timeline label {
  font-size: 0.8rem;
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
  -webkit-transition: .4s;
  transition: .4s;
}

.slider:before {
  position: absolute;
  content: "";
  height: 17px;
  width: 17px;
  left: 5px;
  bottom: 4px;
  background-color: white;
  -webkit-transition: .4s;
  transition: .4s;
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
</style>