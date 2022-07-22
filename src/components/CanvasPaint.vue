<template>
  <div>
    <canvas id="canavs" class="draw" ref="canvas"></canvas>
    <div class="color-panel panel" id="color-panel" ref="coloPanel">
      <li data-color="#F50C0C" style="--color: #f50c0c"></li>
      <li data-color="#FFEB3B" style="--color: #ffeb3b"></li>
      <li data-color="#43CF7C" style="--color: #43cf7c"></li>
      <li data-color="#2A82E4" style="--color: #2a82e4"></li>
      <li data-color="#808080" style="--color: #808080"></li>
      <li data-color="#000" style="--color: #000"></li>
    </div>
    <div class="tools">
      <li
        id="color"
        class="tool-item"
        style="--selected-color: #f50c0c"
        ref="color"
      >
        <div class="color"></div>
      </li>
      <li id="pencil" class="tool-item active" ref="pencil">
        <i class="ri-pencil-fill"></i>
      </li>
      <li id="eraser" class="tool-item" ref="eraser">
        <i class="ri-eraser-fill"></i>
      </li>
      <li id="delete" class="tool-item" ref="delete">
        <i class="ri-delete-bin-fill"></i>
      </li>
      <a href="" download="draw" target="_blank" id="download" ref="download">
        <li class="tool-item">
          <i class="ri-download-2-fill"></i>
        </li>
      </a>
    </div>
  </div>
</template>

<script setup lang="ts">
import { getCurrentInstance, onMounted } from "vue";
let refs;
let canvas: HTMLCanvasElement;
let a: HTMLAnchorElement;
let pencil: HTMLLIElement;
let eraser: HTMLLIElement;
let deleteBtn: HTMLLIElement;
let coloPanel: HTMLDivElement;
let colorBtn: HTMLLIElement;
let ctx: CanvasRenderingContext2D;
let startPos: { x: number | undefined; y: number | undefined } = {
  x: undefined,
  y: undefined,
};
let isPainting = false;
let isErasering = false;
let brushColor = "#f50c0c";
let action = "draw";

interface drawLineType {
  startX: number;
  startY: number;
  endX: number;
  endY: number;
  color: string;
}
onMounted(() => {
  let { $refs } = (getCurrentInstance() as any).proxy;
  refs = $refs;
  canvas = refs.canvas;
  a = refs.download;
  pencil = refs.pencil;
  eraser = refs.eraser;
  deleteBtn = refs.delete;
  coloPanel = refs.coloPanel;
  colorBtn = refs.color;
  if (canvas) ctx = canvas.getContext("2d") as CanvasRenderingContext2D;
  initCanvas();

  canvas.addEventListener("mousedown", function (event) {
    startPos.x = event.offsetX;
    startPos.y = event.offsetY;
    isPainting = true;
  });

  canvas.addEventListener("mousemove", function (event) {
    const endX = event.offsetX;
    const endY = event.offsetY;
    if (
      isPainting &&
      typeof startPos.x === "number" &&
      typeof startPos.y === "number"
    ) {
      if (isErasering && action === "eraser") {
        ctx.clearRect(endX - 5, endY - 5, 25, 25);
      } else {
        drawLine({
          startX: startPos.x,
          startY: startPos.y,
          endX,
          endY,
          color: brushColor,
        });
        startPos.x = endX;
        startPos.y = endY;
      }
    }
  });

  canvas.addEventListener("mouseup", function (event) {
    isPainting = false;
    startPos = { x: undefined, y: undefined };
    enableDownload(canvas);
  });

  pencil.addEventListener("click", function () {
    action = "draw";
    isErasering = false;
    isPainting = true;
    canvas.className = action;
    this.classList.add("active");
    eraser.classList.remove("active");
  });
  eraser.addEventListener("click", function () {
    action = "eraser";
    isErasering = true;
    isPainting = false;
    canvas.className = action;
    this.classList.add("active");
    pencil.classList.remove("active");
  });
  deleteBtn.addEventListener("click", function () {
    ctx.fillStyle = "#f0f0f0";
    ctx.fillRect(0, 0, canvas.width, canvas.height);
  });

  colorBtn.addEventListener("click", toggleColorPanel);

  //  selected color
  coloPanel.addEventListener("click", (event: MouseEvent) => {
    if ((event.target as any).nodeName.toLowerCase() === "li") {
      toggleColorPanel();
      brushColor = (event.target as any).dataset.color;
      colorBtn.style.setProperty(
        "--selected-color",
        (event.target as any).dataset.color
      );
    }
  });
});

function initCanvas() {
  canvas.width = document.documentElement.offsetWidth;
  canvas.height = document.documentElement.offsetHeight;
  enableDownload(canvas);
}

function enableDownload(canavs: HTMLCanvasElement) {
  a.href = canavs.toDataURL();
}

function toggleColorPanel() {
  if (coloPanel.className.includes("show")) {
    coloPanel.classList.remove("show");
    coloPanel.classList.add("hide");
  } else {
    coloPanel.classList.remove("hide");
    coloPanel.classList.add("show");
  }
}

// canvas draw color
function drawLine({
  startX,
  startY,
  endX,
  endY,
  color = brushColor,
}: drawLineType) {
  // start drawing
  ctx.beginPath();

  // line attributes
  ctx.lineWidth = 2;
  ctx.strokeStyle = color;
  ctx.lineCap = "round";
  // start position
  ctx.moveTo(startX, startY);
  // end position
  ctx.lineTo(endX, endY);
  // draw the line
  ctx.stroke();
  // end drawing
  ctx.closePath();
}
</script>

<style lang="scss" scoped>
* {
  margin: 0;
  box-sizing: border-box;
}
body {
  height: 100vh;
}
li {
  list-style-type: none;
}
a {
  color: #000;
  text-decoration: none;
}
canvas {
  position: absolute;
  background-color: #f0f0f0;
  cursor: url("../assets/img/pencil-black.svg"), auto;
}

canvas.eraser {
  cursor: url("../assets/img/eraser-fill.svg"), auto;
}

.tools {
  width: 312px;
  height: 72px;
  border-radius: 36px;
  position: absolute;
  bottom: 10%;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  background-color: #fff;
  box-shadow: 0 12px 20px -8px rgba(190, 190, 190, 0.2);
}

.tool-item {
  width: 64px;
  height: 100%;
  flex: 1;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 36px;
  cursor: pointer;
  transition: all 0.2s ease;
}
.tool-item .color {
  width: 36px;
  height: 36px;
  border-radius: 50%;
  background-color: var(--selected-color);
}
.tool-item.active {
  color: #550af7;
  font-size: 42px;
  transform: translateY(-8px);
}
.panel {
  height: 64px;
  padding: 8px 12px;
  border: 1px solid #eee;
  position: absolute;
  bottom: 17%;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  opacity: 0;
  align-items: center;
  background-color: #fff;
  border-radius: 32px;
}
.panel li {
  width: 36px;
  height: 36px;
  border-radius: 50%;
  background-color: var(--color);
  cursor: pointer;
}
.panel li:not(:last-child) {
  margin-right: 8px;
}
.panel.show {
  animation: show 0.25s linear forwards;
}
.panel.hide {
  animation: hide 0.25s linear both;
}

@keyframes show {
  from {
    opacity: 0;
  }
  50% {
    opacity: 0.5;
    bottom: 10%;
  }
  to {
    bottom: 17%;
    opacity: 1;
  }
}
@keyframes hide {
  from {
    bottom: 17%;
    opacity: 1;
  }
  50% {
    opacity: 0.5;
    bottom: 10%;
  }
  to {
    bottom: 10%;
    opacity: 0;
  }
}
</style>
