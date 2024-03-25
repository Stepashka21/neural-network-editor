<template>
  <div class="app">
    <div class="leftPanel">
      <h3 class="name">
        <span>{{ projectName }}</span>
      </h3>
      <div class="listLayers">
        <h3 class="lay">Слои</h3>
        <div class="listsLay">
          <span class="spans">a</span>
        </div>
      </div>
      <div class="infoElement"></div>
    </div>
    <div class="rightPanel">
      <div class="panelInstruments">
        <div>
          <button @click="clearCanvas" style="margin-left: 10px">
            Очистить холст
          </button>
          <button
            id="pencilBtn"
            @click="changeMode('pencil')"
            style="margin-left: 10px"
          >
            Карандаш
          </button>
          <button
            id="zoomBtn"
            @click="changeMode('zoom')"
            style="margin-left: 10px"
          >
            Фигуры
          </button>
          <button
            id="handBtn"
            @click="changeMode('hand')"
            style="margin-left: 10px"
          >
            Вставить изображение
          </button>
        </div>
        <div class="infoPositions">
          <img
            src="../../src/assets/pngwing_5.png"
            alt="Позиция"
            style="max-height: 21px"
          />
          <span id="mousePosition" style="color: black">{{
            mousePosition
          }}</span>
        </div>
      </div>
      <canvas ref="canvas" width="1660" height="730"></canvas>
      <div class="parameters">
        <div class="query">
          <div class="choiceQuery">
            <button id="selection-1" @click="querySec1()">
              Запрос на генерацию изображения
            </button>
            <button id="selection-2" @click="querySec2()">
              Негативный запрос
            </button>
          </div>
          <div>
            <textarea v-show="textarea1" id="textarea-1">Введите Ваш запрос...</textarea>
            <textarea v-show="textarea2" id="textarea-2">Введите Ваш запрос...</textarea>
          </div>
        </div>
        <div class="settings">
          <div class="options">
            <button class="showDialog" @click="openDialog">
              Дополнительно
            </button>
          </div>
          <div class="settingsImg"></div>
        </div>
        <div class="infoProject">
          <div style="width: 100%; height: 2vh">
            <div class="loader-wrapper">
              <div class="loader-text">{{ loaderProgress }}%</div>
              <div class="loader"></div>
            </div>
          </div>
        </div>
      </div>
      <dialog ref="diaOptions">
        <span>dvsndfsfbnlvnsdfnvskdjfvd</span>
        <button class="closeDialog" @click="closeDialog()">exit</button>
      </dialog>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      projectName: "WindowEditor",
      mousePosition: { x: 0, y: 0 },
      textarea1: true,
      textarea2: false,
      loaderProgress: 0,
      painting: false,
      startX: 0,
      startY: 0,
      offsetX: 0,
      offsetY: 0,
      dragging: false,
      mode: "hand",
    };
  },
  mounted() {
    this.projectName = new URLSearchParams(window.location.search).get(
      "projectName"
    );
    this.setupCanvas();
    this.simulateLoading();
  },
  methods: {
    setupCanvas() {
      const canvas = this.$refs.canvas;
      const ctx = canvas.getContext("2d");
      console.log(ctx);
      canvas.addEventListener("mousedown", this.handleMouseDown);
      canvas.addEventListener("mousemove", this.draw);
      canvas.addEventListener("mouseup", this.handleMouseUp);
      canvas.addEventListener("mouseleave", this.handleMouseLeave);
    },
    simulateLoading() {
      const interval = setInterval(() => {
        if (this.loaderProgress >= 100) {
          clearInterval(interval);
        } else {
          this.loaderProgress += 1;
        }
      }, 60);
    },
    clearCanvas() {
      const canvas = this.$refs.canvas;
      const ctx = canvas.getContext("2d");
      ctx.clearRect(0, 0, canvas.width, canvas.height);
    },
    handleMouseDown(e) {
      // Сохраняем начальные координаты нажатия
      this.startX = e.clientX;
      this.startY = e.clientY;
    },

    draw(e) {
      // Проверяем, нажата ли кнопка мыши
      if (this.painting) {
        const canvas = this.$refs.canvas;
        const ctx = canvas.getContext("2d");

        // Рисуем линию от предыдущих координат до текущих
        ctx.beginPath();
        ctx.moveTo(this.startX, this.startY);
        ctx.lineTo(e.clientX, e.clientY);
        ctx.stroke();

        // Обновляем начальные координаты для следующей точки
        this.startX = e.clientX;
        this.startY = e.clientY;
      }
    },
    handleMouseUp() {
      // Завершаем рисование
      this.painting = false;
    },
    handleMouseLeave() {
      // Если курсор ушел за пределы холста, завершаем рисование
      this.painting = false;
    },
    changeMode(newMode) {
      this.mode = newMode;
    },
    querySec1() {
      this.textarea2 = true;
      this.textarea1 = false;
    },
    querySec2() {
      this.textarea2 = false;
      this.textarea1 = true;
    },
    openDialog() {
      console.log("sadadadasd");
      this.$refs.diaOptions.showModal();
    },
    closeDialog() {
      this.$refs.diaOptions.close();
    },
  },
};
</script>

<style scoped>
body {
  display: flex;
  flex-direction: row;
  align-items: flex-start;
  margin: 0;
  padding: 0;
  background-color: #464646;
}
.leftPanel {
  width: 13%;
  margin: 10px 25px 10px 10px;
}
.name {
  margin: 0;
  border-radius: 12px;
  height: 32px;
  background-color: #868686;
  color: #000000;
  display: flex;
  align-items: center;
  justify-content: center;
}
.listLayers {
  margin-top: 25px;
  background-color: #2c2c2c;
  height: 52vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  overflow-y: auto;
  border-radius: 8px 8px 13px 13px;
}
.lay {
  color: #ffffff;
  margin: 0;
  padding: 0;
}
.listsLay {
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  width: 100%;
}
.spans {
  margin: 5px 5px;
  padding: 0 10px;
  background-color: #868686;
  border-radius: 13px;
}
.infoElement {
  height: 38vh;
  margin-top: 15px;
  background-color: #2c2c2c;
  border-radius: 6px;
}

.rightPanel {
  display: flex;
  flex-direction: column;
  margin: 10px 13px 13px 0px;
  max-width: 85%;
}
.panelInstruments {
  display: flex;
  align-items: center;
  flex-direction: row;
  justify-content: space-between;
  background-color: #2c2c2c;
  width: 100%;
  height: 32px;
  border-radius: 10px 10px 0 0;
}
.infoPositions {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-between;
  border-radius: 10px;
  background-color: #e9e9e9;
  margin-right: 10px;
  padding: 3px 10px;
}
.spanss {
  margin: 5px 5px;
  padding: 0 10px;
  background-color: #868686;
}

canvas {
  border: 1px solid black;
  border-radius: 0 0 21px 21px;
  background-color: white;
  margin: 0;
  padding: 0;
}
.parameters {
  display: flex;
  flex-direction: row;
}
.query {
  display: flex;
  flex-direction: column;
  width: 39%;
  margin-right: 7px;
}
.choiceQuery {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-between;
  width: 100%;
}
#selection-1 {
  height: 4vh;
  width: 52%;
  border-radius: 12px;
  margin: 8px 0;
  background-color: #ffffff;
  color: #000000;
  border: none;
}
#selection-2 {
  height: 4vh;
  width: 44%;
  border-radius: 12px;
  margin: 8px 0;
  background-color: #000000;
  color: #ffffff;
  border: none;
}
#textarea-1 {
  padding-left: 10px;
  border-radius: 12px;
  resize: none;
  height: 21vh;
  width: 98%;
  background-color: #ffffff;
  color: #000000;
  display: block;
}
#textarea-2 {
  padding-left: 10px;
  border-radius: 12px;
  resize: none;
  height: 21vh;
  width: 98%;
  background-color: #000000;
  color: #ffffff;
  display: none;
}
button:hover {
  cursor: pointer;
}
.settings {
  display: flex;
  width: 38%;
  flex-direction: column;
  align-items: flex-end;
}
.showDialog {
  height: 4vh;
  border-radius: 12px;
  margin: 8px 0;
  padding: 0 59px;
  background-color: #000000;
  color: #ffffff;
  border: none;
}
.settingsImg {
  padding: 5px 0 0 10px;
  border-radius: 12px;
  resize: none;
  height: 21vh;
  width: 98%;
  background-color: #ffffff;
  color: #000000;
}
.infoProject {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 22%;
  height: 26vh;
  margin: 8px 0px 0px 5px;
  background-color: aqua;
}
.loader {
  display: block;
  position: relative;
  height: 16px;
  width: 100%;
  border: 3px solid #000;
  border-radius: 12px;
  overflow: hidden;
}
.loader:after {
  content: "";
  position: absolute;
  padding: 7px;
  top: 1px;
  left: 2px;
  height: 0%;
  width: 0px;
  border-radius: 12px;
  background: #000000;
  animation: 6s prog ease-in forwards;
}
.loader-text {
  font-size: 14px;
  color: #000;
}
@keyframes prog {
  to {
    width: 94%;
  }
}
.loader-text::before {
  /* Отображаем текст с процентом, который задается переменной --progress */
  content: attr(data-progress, "%");
}

/*   Open state of the dialog  */
dialog[open] {
  opacity: 1;
  transform: scaleY(1);
}

/*   Closed state of the dialog   */
dialog {
  opacity: 0;
  transform: scaleY(0);
  transition: opacity 0.7s ease-out, transform 0.7s ease-out,
    overlay 0.7s ease-out allow-discrete, display 0.7s ease-out allow-discrete;
  /* Equivalent to
  transition: all 0.7s allow-discrete; */
}

/*   Before-open state  */
/* Needs to be after the previous dialog[open] rule to take effect,
    as the specificity is the same */
@starting-style {
  dialog[open] {
    opacity: 0;
    transform: scaleY(0);
  }
}

/* Transition the :backdrop when the dialog modal is promoted to the top layer */
dialog::backdrop {
  background-color: rgb(0 0 0 / 0%);
  transition: display 0.7s allow-discrete, overlay 0.7s allow-discrete,
    background-color 0.7s;
  /* Equivalent to
  transition: all 0.7s allow-discrete; */
}

dialog[open]::backdrop {
  background-color: rgb(0 0 0 / 25%);
}

/* This starting-style rule cannot be nested inside the above selector
because the nesting selector cannot represent pseudo-elements. */

@starting-style {
  dialog[open]::backdrop {
    background-color: rgb(0 0 0 / 0%);
  }
}
</style>
