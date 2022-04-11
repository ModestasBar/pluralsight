<template>
  <div class="content">
    <button class="add-to-cart" @click="addToCart">Add to card</button>
    <div class="top-row">
      <div class="top part">
        <div class="robot-name">
          {{ selectedRobots.head.title }}
          <span v-if="selectedRobots.head.onSale" class="sale">Sale!</span>
        </div>
        <img :src="selectedRobots.head.src" alt="head" title="head" />
        <button @click="decreaseNextHead" class="prev-selector">&#9668;</button>
        <button @click="selectNextHead" class="next-selector">&#9658;</button>
      </div>
    </div>
    <div class="middle-row">
      <div class="left part">
        <img :src="selectedRobots.leftArm.src" alt="head" title="left arm" />
        <button @click="decreaseNextLeftArm" class="prev-selector">&#9650;</button>
        <button @click="selectNextLeftArm" class="next-selector">&#9660;</button>
      </div>
      <div class="center part">
        <img :src="selectedRobots.torso.src" alt="head" title="left arm" />
        <button @click="decreaseNextTorso" class="prev-selector">&#9668;</button>
        <button @click="selectNextTorso" class="next-selector">&#9658;</button>
      </div>
      <div class="right part">
        <img :src="selectedRobots.rightArm.src" alt="head" title="left arm" />
        <button @click="decreaseNextRightArm" class="prev-selector">&#9650;</button>
        <button @click="selectNextRightArm" class="next-selector">&#9660;</button>
      </div>
    </div>
    <div class="bottom-row">
      <div class="bottom part">
        <img :src="selectedRobots.base.src" alt="head" title="left arm" />
        <button @click="decreaseNextBottom" class="prev-selector">&#9668;</button>
        <button @click="selectNextBottom" class="next-selector">&#9658;</button>
      </div>
    </div>
    <div>
        <table>
            <thead>
                <tr>
                    <th>Robot</th>
                    <th class="cost">Cost</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(robot, index) in cart" :key="index">
                    <td>{{robot.head.title}}</td>
                    <td class="cost">{{robot.cost}}</td>
                </tr>
            </tbody>
        </table>
    </div>
  </div>
</template>

<script>
import parts from '../data/parts';

function getPreviousValidIndex(index, length) {
  const deprecatedIndex = index - 1;
  return deprecatedIndex < 0 ? length - 1 : deprecatedIndex;
}

function getNextValidIndex(index, length) {
  const incrementedIndex = index + 1;
  return incrementedIndex > length - 1 ? 0 : incrementedIndex;
}

export default {
  name: 'RobotBuilder',

  data() {
    return {
      cart: [],
      parts,
      selectNextHeadIndex: 0,
      selectNextLeftArmIndex: 0,
      selectNextRightArmIndex: 0,
      selectNextTorsoIndex: 0,
      selectNextBottomIndex: 0,
    };
  },

  computed: {
    selectedRobots() {
      return {
        head: parts.heads[this.selectNextHeadIndex],
        leftArm: parts.arms[this.selectNextLeftArmIndex],
        torso: parts.torsos[this.selectNextTorsoIndex],
        rightArm: parts.arms[this.selectNextRightArmIndex],
        base: parts.bases[this.selectNextBottomIndex],
      };
    },
  },

  methods: {
    selectNextHead() {
      this.selectNextHeadIndex = getNextValidIndex(this.selectNextHeadIndex, parts.heads.length);
    },
    decreaseNextHead() {
      this.selectNextHeadIndex = getPreviousValidIndex(
        this.selectNextHeadIndex,
        parts.heads.length,
      );
    },
    selectNextLeftArm() {
      this.selectNextLeftArmIndex = getNextValidIndex(
        this.selectNextLeftArmIndex,
        parts.arms.length,
      );
    },
    decreaseNextLeftArm() {
      this.selectNextLeftArmIndex = getPreviousValidIndex(
        this.selectNextLeftArmIndex,
        parts.arms.length,
      );
    },
    selectNextTorso() {
      this.selectNextTorsoIndex = getNextValidIndex(this.selectNextTorsoIndex, parts.torsos.length);
    },
    decreaseNextTorso() {
      this.selectNextTorsoIndex = getPreviousValidIndex(
        this.selectNextTorsoIndex,
        parts.torsos.length,
      );
    },
    selectNextRightArm() {
      this.selectNextRightArmIndex = getNextValidIndex(
        this.selectNextRightArmIndex,
        parts.arms.length,
      );
    },
    decreaseNextRightArm() {
      this.selectNextRightArmIndex = getPreviousValidIndex(
        this.selectNextRightArmIndex,
        parts.arms.length,
      );
    },
    selectNextBottom() {
      this.selectNextBottomIndex = getNextValidIndex(
        this.selectNextBottomIndex,
        parts.bases.length,
      );
    },
    decreaseNextBottom() {
      this.selectNextBottomIndex = getPreviousValidIndex(
        this.selectNextBottomIndex,
        parts.bases.length,
      );
    },
    addToCart() {
      const robot = this.selectedRobots;
      console.log(robot);
      const cost = robot.head.cost
        + robot.leftArm.cost
        + robot.rightArm.cost
        + robot.torso.cost
        + robot.base.cost;

      this.cart = [...this.cart, { ...robot, cost }];
    },
  },
};
</script>

<style>
.part {
  position: relative;
  width: 165px;
  height: 165px;
  border: 3px solid #aaa;
}
.part img {
  width: 165px;
}
.top-row {
  display: flex;
  justify-content: space-around;
}
.middle-row {
  display: flex;
  justify-content: center;
}
.bottom-row {
  display: flex;
  justify-content: space-around;
  border-top: none;
}
.head {
  border-bottom: none;
}
.left {
  border-right: none;
}
.right {
  border-left: none;
}
.left img {
  transform: rotate(-90deg);
}
.right img {
  transform: rotate(90deg);
}
.bottom {
  border-top: none;
}
.prev-selector {
  position: absolute;
  z-index: 1;
  top: -3px;
  left: -28px;
  width: 25px;
  height: 171px;
}
.next-selector {
  position: absolute;
  z-index: 1;
  top: -3px;
  right: -28px;
  width: 25px;
  height: 171px;
}
.center .prev-selector,
.center .next-selector {
  opacity: 0.8;
}
.left .prev-selector {
  top: -28px;
  left: -3px;
  width: 144px;
  height: 25px;
}
.left .next-selector {
  top: auto;
  bottom: -28px;
  left: -3px;
  width: 144px;
  height: 25px;
}
.right .prev-selector {
  top: -28px;
  left: 24px;
  width: 144px;
  height: 25px;
}
.right .next-selector {
  top: auto;
  bottom: -28px;
  left: 24px;
  width: 144px;
  height: 25px;
}
.right .next-selector {
  right: -3px;
}

.robot-name {
  position: absolute;
  top: -25px;
  text-align: center;
  width: 100%;
}
.sale {
  color: red;
}
.content {
  position: relative;
}
.add-to-cart {
  position: absolute;
  right: 35px;
  width: 220px;
  padding: 3px;
  font-size: 16px;
}
td, th {
    text-align: left;
    padding: 5px;
    padding-right: 20px;
}
.cost{
    text-align: right;
}
</style>
