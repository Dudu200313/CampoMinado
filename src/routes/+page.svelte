<script>
  import { onMount } from "svelte";
  import { row, col } from "../store.js";
  import { goto } from "$app/navigation";
  import "../app.css";

  let nodes = 10;
  let waveHeight = 150;
  let colours = ["#fff", "#44c2ce"];
  let canvases = [
    {
      id: "canvas",
      waves: [],
    },
    {
      id: "canvas-bottom",
      waves: [],
    },
  ];

  function init() {
    canvases.forEach((canvasObj) => {
      let canvas = document.getElementById(canvasObj.id);
      canvasObj.element = canvas;
      canvasObj.context = canvas.getContext("2d");
      resizeCanvas(canvas);

      for (let i = 0; i < 3; i++) {
        canvasObj.waves.push(new wave(colours[i], 1, nodes, canvas.width));
      }

      update(canvasObj);
    });
  }

  function update(canvasObj) {
    let ctx = canvasObj.context;
    let cvs = canvasObj.element;

    ctx.globalCompositeOperation = "source-over";
    ctx.fillRect(0, 0, cvs.width, cvs.height);
    ctx.globalCompositeOperation = "screen";

    for (let i = 0; i < canvasObj.waves.length; i++) {
      for (let j = 0; j < canvasObj.waves[i].nodes.length; j++) {
        bounce(canvasObj.waves[i].nodes[j], cvs);
      }
      drawWave(canvasObj.waves[i], ctx, cvs);
    }

    requestAnimationFrame(() => {
      update(canvasObj);
    });
  }

  function wave(colour, lambda, nodes, cvsWidth) {
    this.colour = colour;
    this.lambda = lambda;
    this.nodes = [];

    for (let i = 0; i <= nodes + 2; i++) {
      let temp = [((i - 1) * cvsWidth) / nodes, 0, Math.random() * 200, 0.3];
      this.nodes.push(temp);
    }
  }

  function bounce(nodeArr, cvs) {
    nodeArr[1] = (waveHeight / 2) * Math.sin(nodeArr[2] / 20) + cvs.height / 2;
    nodeArr[2] = nodeArr[2] + nodeArr[3];
  }

  function drawWave(obj, ctx, cvs) {
    let diff = function (a, b) {
      return (b - a) / 2 + a;
    };

    ctx.fillStyle = obj.colour;
    ctx.beginPath();
    ctx.moveTo(0, cvs.height);
    ctx.lineTo(obj.nodes[0][0], obj.nodes[0][1]);

    for (let i = 0; i < obj.nodes.length; i++) {
      if (obj.nodes[i + 1]) {
        ctx.quadraticCurveTo(
          obj.nodes[i][0],
          obj.nodes[i][1],
          diff(obj.nodes[i][0], obj.nodes[i + 1][0]),
          diff(obj.nodes[i][1], obj.nodes[i + 1][1])
        );
      } else {
        ctx.lineTo(obj.nodes[i][0], obj.nodes[i][1]);
        ctx.lineTo(cvs.width, cvs.height);
      }
    }

    ctx.closePath();
    ctx.fill();
  }

  function resizeCanvas(canvas, width, height) {
    if (width && height) {
      canvas.width = width;
      canvas.height = height;
    } else {
      if (window.innerWidth > 1920) {
        canvas.width = window.innerWidth;
      } else {
        canvas.width = 1920;
      }
      canvas.height = waveHeight;
    }
  }

  onMount(init);

  function startGame() {
    goto("game");
  }
</script>

<div class="canvas-wrap top">
  <canvas id="canvas" class="test" />
</div>

<div class="canvas-wrap bottom">
  <canvas id="canvas-bottom" class="test" />
</div>

<div class="wrap">
  <input type="checkbox" id="form_switch" style="display: none;" />
  <div class="flipcard">
    <div class="flipcard-inner">
      <div class="flipcard-front">
        <div class="minesweeper-container">
          <h1 class="titulo">Campo Minado</h1>
          <button id="row-button" class="input-label">Rows</button>
          <input id="row-input" class="input-field" bind:value={$row} />

          <button id="col-button" class="input-label">Columns</button>
          <input id="col-input" class="input-field" bind:value={$col} />

         

          <button class="start-button" on:click={() => startGame($row, $col)}
            >Start Game</button
          >
           <a href="/apresentacao" class="start-button">Apresentação</a>
        </div>
      </div>
    </div>
  </div>
</div>

<style>
  * {
    box-sizing: border-box;
  }

  body {
    margin: 0;
    padding: 0;
    font-family: Arial, sans-serif; /* Adicione a fonte desejada */
  }

  .canvas-wrap {
    max-width: 100%;
    overflow: hidden;
    position: absolute;
  }

  canvas {
    display: block;
  }

  .top {
    top: 0;
  }

  .bottom {
    bottom: 0;
    transform: rotate(180deg);
  }

  .wrap {
    width: 350px;
    margin: 45px auto;
    padding: 20px; /* Aumentei o padding para dar mais espaço entre os elementos */
  }

  .minesweeper-container {
    text-align: center;
  }

  .titulo {
    background-color: white;
    padding: 10px;
    margin: 10px;
  }

  .input-label,
  .input-field,
  .start-button {
    display: block;
    width: 100%;
    margin-bottom: 10px;
  }

  .input-label {
    background: #69d2e7;
    color: #fff;
    padding: 10px;
    cursor: pointer;
    transition: background-color 0.3s;
  }

  .input-label:hover {
    background: #69c8e7;
  }

  .input-field {
    padding: 10px;
    border: 1px solid #ddd;
    background: #fafafa;
  }

  .start-button {
    background: #2c91d8;
    color: #fff;
    padding: 10px;
    text-transform: uppercase;
    border: none;
    cursor: pointer;
    transition: background-color 0.3s;
  }

  .start-button:hover {
    background: #2c91a8;
  }

  .flipcard {
    background-color: transparent;
    width: 350px;
    perspective: 1000px;
  }

  .flipcard-inner {
    position: relative;
    width: 100%;
    height: 100%;
    text-align: center;
    transition: transform 0.8s;
    transform-style: preserve-3d;
  }

  .flipcard-front,
  .flipcard-back {
    position: absolute;
    width: 100%;
    height: 100%;
    backface-visibility: hidden;
  }

  .flipcard-front {
    color: black;
  }

  .flipcard-back {
    background-color: dodgerblue;
    transform: rotateY(180deg);
  }

  input[type="checkbox"]:checked + .flipcard .flipcard-inner {
    transform: rotateY(180deg);
  }

  .label-highlight {
    color: blue;
  }

  .start-button {
        display: inline-block;
        padding: 12px 24px;
        background-color: #007bff;
        color: #fff;
        font-size: 18px;
        text-decoration: none;
        border-radius: 5px;
        transition: background-color 0.3s;
        cursor: pointer;
    }

    .start-button:hover {
        background-color: #0e78e9;
    }

</style>
