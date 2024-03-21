<script setup lang="ts">
  import { type Ref, ref } from "vue";

  const cells: Array<Number[]> = [
    [72, 71, 70, 69, 68, 67, 66, 65, 64],
    [55, 56, 57, 58, 59, 60, 61, 62, 63],
    [54, 53, 52, 51, 50, 49, 48, 47, 46],
    [37, 38, 39, 40, 41, 42, 43, 44, 45],
    [36, 35, 34, 33, 32, 31, 30, 29, 28],
    [19, 20, 21, 22, 23, 24, 25, 26, 27],
    [18, 17, 16, 15, 14, 13, 12, 11, 10],
    [1, 2, 3, 4, 5, 6, 7, 8, 9],
  ];
  const backgroundImageUrl = ref("");
  // const backgroundImageUrlTwo = ref("");

  const onFileChange = (event: any) => {
    const file = event.target.files[0];
    backgroundImageUrl.value = URL.createObjectURL(file);
  };
  // const onFileChangeTwo = (event : any) => {
  //     const file = event.target.files[0];
  //     backgroundImageUrlTwo.value = URL.createObjectURL(file);
  // };

  const currentCell: Ref<Number> = ref(68);
  // const currentCellTwo : Ref<Number> = ref(68)
  const focusedCell: Ref<Number> = ref(68);

  const isMoving: Ref<Boolean> = ref(false);
  // const isMovingTwo : Ref<Boolean> = ref(false)

  const gameMode: Ref<Boolean> = ref(false);
  const startCell: Ref<Number> = ref(0);
  const finishCell: Ref<Number> = ref(0);

  const moveChip = (item: Number) => {
    focusedCell.value = item;
    // if (isMovingTwo.value) {
    //   isMovingTwo.value = !isMovingTwo.value;
    // }
    isMoving.value = !isMoving.value;
  };

  // const moveChipTwo = (item : Number) => {
  //     focusedCell.value = item
  //     if (isMoving.value) {
  //         isMoving.value = !isMoving.value
  //     }
  //     isMovingTwo.value = !isMovingTwo.value
  // }

  const cellClick = (item: Number) => {
    if (isMoving.value) {
      currentCell.value = item;
      if (gameMode.value) {
        gameModeRules(item);
      }
      isMoving.value = !isMoving.value;
    }
    focusedCell.value = item;
  };

  const gameModeRules = (item: Number) => {
    switch (item) {
      case 10:
        timeoutSwitchOn(item, 23);
        break;
      case 12:
        timeoutSwitchOn(item, 8);
        break;
      case 16:
        timeoutSwitchOn(item, 4);
        break;
      case 17:
        timeoutSwitchOn(item, 69);
        break;
      case 20:
        timeoutSwitchOn(item, 32);
        break;
      case 22:
        timeoutSwitchOn(item, 60);
        break;
      case 24:
        timeoutSwitchOn(item, 7);
        break;
      case 27:
        timeoutSwitchOn(item, 41);
        break;
      case 28:
        timeoutSwitchOn(item, 50);
        break;
      case 29:
        timeoutSwitchOn(item, 6);
        break;
      case 37:
        timeoutSwitchOn(item, 66);
        break;
      case 44:
        timeoutSwitchOn(item, 9);
        break;
      case 45:
        timeoutSwitchOn(item, 67);
        break;
      case 46:
        timeoutSwitchOn(item, 62);
        break;
      case 52:
        timeoutSwitchOn(item, 35);
        break;
      case 54:
        timeoutSwitchOn(item, 68);
        break;
      case 55:
        timeoutSwitchOn(item, 3);
        break;
      case 61:
        timeoutSwitchOn(item, 13);
        break;
      case 63:
        timeoutSwitchOn(item, 2);
        break;
      case 72:
        timeoutSwitchOn(item, 51);
        break;
      default:
        break;
    }
  };

  const timeoutSwitchOn = (start: Number, finish: Number) => {
    startCell.value = start;
    finishCell.value = finish;
    setTimeout(() => {
      currentCell.value = finish;
      focusedCell.value = finish;
      startCell.value = 0;
      finishCell.value = 0;
    }, 3000);
  };
</script>

<template>
  <div class="game">
    <div class="game-field">
      <img src="../assets/background2.jpg" alt="background" />
      <div class="cells">
        <div class="cells-row" v-for="row in cells">
          <div
            class="cells-row__item"
            v-for="item in row"
            :class="{
              'cell-active': focusedCell == item,
              'start-active': startCell == item,
              'finish-active': finishCell == item,
            }"
            @click="cellClick(item)"
          >
            <div
              v-if="item == currentCell"
              class="chip"
              :class="{
                'chip-active': isMoving,
              }"
              :style="{
                'background-image': 'url(' + backgroundImageUrl + ')',
              }"
              @click.stop="moveChip(item)"
            >
              <template v-if="!backgroundImageUrl">
                <input id="fileInput" type="file" @change="onFileChange" />
              </template>
            </div>
            <!-- <div
              v-if="item == currentCellTwo"
              class="chip"
              :class="{
                'chip-active': isMovingTwo,
                'small-chip-right': currentCell == currentCellTwo,
              }"
              :style="{
                'background-image': 'url(' + backgroundImageUrlTwo + ')',
              }"
              @click.stop="moveChipTwo(item)"
            >
              <template v-if="!backgroundImageUrlTwo">
                <input id="fileInput" type="file" @change="onFileChangeTwo" />
              </template>
            </div> -->
          </div>
        </div>
      </div>
    </div>
    <!-- <div class="game-dashboard">
            <h2>Текущая клекта {{ focusedCell }}</h2>
            <br>
            <h3> Название клетки {{ focusedCell }}</h3>
            <br>
            <h6>Lorem, ipsum dolor sit amet consectetur adipisicing elit. Saepe, vero! Provident, dolor animi. Modi fugit exercitationem, suscipit reiciendis porro ex!</h6>
            <br>
            <b>Интерактивный режим:</b> <input type="checkbox" v-model="gameMode" >
        </div> -->
  </div>
</template>

<style scoped>
  .game {
    display: flex;
    justify-content: center;
    width: 100vw;
    background-color: rgba(245, 243, 199, 0.724);
  }

  .game-field {
    position: relative;
    border-right: 1px solid black;
    border-left: 1px solid black;
  }

  .game-field img {
    position: relative;
    height: calc(100vh - 5px);
    z-index: 0;
  }

  .game-dashboard {
    padding: 10px;
    flex-basis: 45%;
  }

  .cell {
    height: 71px;
    width: 71px;
  }

  .cells {
    position: absolute;
    top: 0;
    left: 0;
    display: flex;
    flex-wrap: wrap;
    padding: 11px 9px;
  }

  .cells-row {
    display: flex;
    width: 100%;
  }

  .cells-row__item {
    cursor: pointer;
    position: relative;
    width: 89px;
    height: 89px;
  }

  .chip {
    cursor: pointer;
    position: absolute;
    height: 60px;
    width: 60px;
    background-color: rgb(247, 234, 234);
    background-position: center;
    background-repeat: no-repeat;
    background-size: cover;
    border-radius: 50%;
    top: 40%;
    left: 50%;
    transform: translate(-50%, -50%);
  }

  .chip-active {
    box-shadow: 0px 0px 5px black;
    animation: 1s linear 0s infinite alternate pulse-default-shadow;
  }

  .start-active {
    box-shadow: 0px 0px 5px green;
    animation: 1s linear 0s infinite alternate pulse-start-shadow;
  }

  .finish-active {
    box-shadow: 0px 0px 5px red;
    animation: 1s linear 0s infinite alternate pulse-finish-shadow;
  }

  .small-chip-left {
    height: 45px;
    width: 45px;
    top: 40%;
    left: 0;
    transform: translateY(-50%);
  }

  .small-chip-right {
    height: 45px;
    width: 45px;
    top: 40%;
    right: 0;
    transform: translateY(-50%);
  }

  #fileInput {
    position: absolute;
    cursor: pointer;
    display: block;
    opacity: 0;
    width: 60px;
    height: 60px;
    z-index: 10;
  }

  @keyframes pulse-finish-shadow {
    100% {
      box-shadow: 0px 0px 60px red;
    }
  }
  @keyframes pulse-start-shadow {
    100% {
      box-shadow: 0px 0px 60px green;
    }
  }
  @keyframes pulse-default-shadow {
    100% {
      box-shadow: 0px 0px 60px black;
    }
  }
</style>
