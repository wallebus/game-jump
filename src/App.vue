<script lang="ts" setup>

// import "./assets/style.css";
import { onMounted, reactive, Ref, ref } from "vue";
import dog_img from "./assets/dog.png";


// Init canvas
const canvas =
  document.querySelector("canvas") ?? document.createElement("canvas");
onMounted(() => {
  canvas.id = "sprit"
  const app = document.querySelector("#app")
  app?.appendChild(canvas);
})

const ctx = canvas.getContext("2d");
const WIDTH = canvas.width = 600
const HEIGHT = canvas.height = 600

// Control the sprit option
const Frames = [
  { name: "idle", frames: 7 },
  { name: 'jump', frames: 7 },
  { name: 'down', frames: 7 },
  { name: 'run', frames: 9 },
  { name: 'dizzy', frames: 11 },
  { name: 'lie', frames: 5 },
  { name: 'roll', frames: 7 },
  { name: 'bite', frames: 7 },
  { name: 'ko', frames: 12 },
  { name: 'getHit', frames: 4 }] as const

type animateName = (typeof Frames)[number]["name"]
const Sprit = {
  width: 6900 / 12,
  height: 5230 / 10,
  frameX: 0,
  frameY: 0,
  Frames,
  staggerFrame: 3,
  gameFrame: 0,
  currentFames(name: animateName) {
    let { frames } =
      this.Frames.find((item, index) => {
        this.frameY = index;
        return item["name"] == name
      }) as { name: string, frames: number }

    return frames
  }
}


const dogAnimate = new Image();
dogAnimate.src = dog_img;

let selected = ref("idle") as Ref<animateName>
function animate() {
  ctx?.clearRect(0, 0, WIDTH, HEIGHT);
  let position = Math.floor(Sprit.gameFrame / Sprit.staggerFrame) % Sprit.currentFames(selected.value);
  Sprit.frameX = Sprit.width * position;
  // (IMAGE,dx,dy,dw,dh,cx,cy,cw,ch)
  // dx 控制裁剪的位置大小，cx 控制显示区域的位置大小
  ctx?.drawImage(
    dogAnimate,
    Sprit.frameX,
    Sprit.height * Sprit.frameY,
    Sprit.width,
    Sprit.height,
    0,
    0,
    Sprit.width,
    Sprit.height
  );

  Sprit.gameFrame++
  requestAnimationFrame(animate);
}

animate();

window.addEventListener("resize", () => {
  let unit: "80vh" | "80vw"

  if (window.innerWidth > window.innerHeight) {
    unit = "80vh"
  } else {
    unit = "80vw"
  }
  canvas.style.width = canvas.style.height = unit
})

</script>
<template >
  <select name="animation" id="animate-select" v-model="selected">
    <option v-for="item in Frames" :value="item.name">{{ item.name }}</option>
  </select>
</template>

<style>
#sprit {
  display: block;

  border: 2px solid rgb(229, 229, 229);
}

@media (width > height) {
  #sprit {
    width: 80vh;
    height: 80vh;
  }
}

#animate-select {
  width: 70px;
  height: 25px;
  margin: 20px 0;
  font-size: medium;
}

#animate-select option {
  text-align: center;
}
</style>