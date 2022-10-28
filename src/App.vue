<script lang="ts" setup>

// import "./assets/style.css";
import { onMounted } from "vue";
import dog_img from "./assets/dog.png";

let canvas: HTMLCanvasElement;

// Init canvas
canvas = document.querySelector("canvas") ?? document.createElement("canvas");
canvas.id = "sprit"
const app = document.querySelector("#app")
app?.appendChild(canvas);

const ctx = canvas.getContext("2d");
const WIDTH = (canvas.width = 600);
const HEIGHT = (canvas.height = 600);

// Control the sprit option
const framesX = [
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

type animateName = (typeof framesX)[number]["name"]
const Sprit = {
  width: 6900 / 12,
  height: 5230 / 10,
  frameX: 0,
  frameY: 0,
  framesX,
  staggerFrame: 2,
  gameFrame: 0,
  currentFames(name: animateName) {
    let { frames } =
      this.framesX.find((item, index) => {
        this.frameY = index;
        return item["name"] == name
      }) as { name: string, frames: number }

    return frames
  }
}


const dogAnimate = new Image();
dogAnimate.src = dog_img;

function animate() {
  ctx?.clearRect(0, 0, WIDTH, HEIGHT);

  // (IMAGE,dx,dy,dw,dh,cx,cy,cw,ch)
  // dx 控制裁剪的位置大小，cx 控制显示区域的位置大小
  ctx?.drawImage(
    dogAnimate,
    Sprit.width * Sprit.frameX,
    Sprit.height * Sprit.frameY,
    Sprit.width,
    Sprit.height,
    0,
    0,
    Sprit.width,
    Sprit.height
  );

  if (Sprit.frameX < Sprit.currentFames("jump") - 1) {
    Sprit.frameX++
  } else { Sprit.frameX = 0 }

  // Sprit.gameFrame++
  requestAnimationFrame(animate);
}

animate();

</script>
<template >

</template>
<style>
#sprit {
  border: 2px solid rgb(229, 229, 229);
}
</style>