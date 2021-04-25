<template>
  <div class="">
    <div
      class="timeline"
      @mouseenter="isXvisible = true"
      @mouseleave="isXvisible = false"
    >
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
           <transition appear name="fade"> <div class="bullet"></div></transition>
            

            <div class="box">
              <transition
                appear
                @enter="enter"
                @before-leave="beforeLeave"
                @leave="leave"
              >
                <div class="box_inner">
                  <p class="date" contentEditable="true">{{ item.formatedDate }}</p>
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
      isVisible: false,
      isXvisible: true,
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
    sortByDate() {
      return this.list
        .sort((a, b) => {
          let dateA = new Date(a.date)
          let dateB = new Date(b.date)
          return dateA - dateB
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
  /* width: 100%; */
  height: 100vh;
  padding: 8.5em 0;
  box-sizing: border-box;
}
.wrap {
  display: flex;
  padding: 1em 0.8em 0.5em;
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
  top: 0;
  left: 0;
  width: 2px;
  height: 100%;
  /* flex: 0 0 3px; */
  background: rgb(209, 209, 209);
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
  border: 4px solid rgb(92, 183, 219);
  background: var(--text-accent);
}
/* .arrowhead{
   position: absolute;
  bottom:-7px;
  left:-2px;
  width: 7px;
  height: 7px;
  border-radius: 30%;
  /* border: 4px solid rgb(92, 183, 219); */
/* background: rgb(209, 209, 209);} */
.box {
  text-align: left;
}
.box_inner {
  /* border: 1px solid rgb(155, 155, 155); */
  border-radius: 4px;
  margin: 0 0 0.5em;
  padding: 0.2em 1em 0.2em;
  background: #fff;
  z-index: 0;
  box-shadow: 02px 01px 10px rgba(190, 190, 190, 0.382);
}
.box_inner::before {
  content: "";
  width: 15px;
  height: 15px;
  position: absolute;
  top: 02px;
  left: -02px;
  background: rgb(255, 255, 255);
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
  color: var(--text);
}
.date {
  padding: 0.1em 0;
  font-weight: 300;
  letter-spacing: 0.5px;
  font-size: 1.4rem;
  margin: 0 0 0.2em;
  color: var(--text);
}
.content {
  padding: 0em 0.5em 0.5em;
  font-size: 1rem;
  color: var(--text);
  font-weight: 400;
}
</style>