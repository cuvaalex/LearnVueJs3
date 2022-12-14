<template>
  <div>
    <div class="content"/>
    <div>
      <button class="add-to-cart" @click="addToCart()">Add to Cart</button>
    </div>
    <div class="top-row">
      <div class="top part" :class="{'sale-border': selectedRobot.head.onSale}">
        <div class="robot-name">
          {{selectedRobot.head.title}}
          <span v-show="selectedRobot.head.onSale" class="sale">Sale</span>
        </div>
        <img alt="" :src="selectedRobot.head.src" title="head"/>
        <button @click="selectPreviousHead()" class="prev-selector">&#9668;</button>
        <button @click="selectNextHead();" class="next-selector">&#9658;</button>
      </div>
    </div>
    <div class="middle-row">
      <div class="left part">
        <img alt="" :src="selectedRobot.leftArm.src" title="left arm"/>
        <button @click="selectPreviousLeftArm()" class="prev-selector">&#9650;</button>
        <button @click="selectNextLeftArm();" class="next-selector">&#9660;</button>
      </div>
      <div class="center part">
        <img alt="" :src="selectedRobot.torsos.src" title="torsos"/>
        <button @click="selectPreviousTorsos()" class="prev-selector">&#9668;</button>
        <button @click="selectNextTorsos();" class="next-selector">&#9658;</button>
      </div>
      <div class="right part">
        <img alt="" :src="selectedRobot.rightArm.src" title="right arm"/>
        <button @click="selectPreviousRightArm()" class="prev-selector">&#9650;</button>
        <button @click="selectNextRightArm();" class="next-selector">&#9660;</button>
      </div>
    </div>
    <div class="bottom-row">
      <div class="bottom part">
        <img alt="" :src="selectedRobot.bases.src" title="bases"/>
        <button @click="selectPreviousBases();" class="prev-selector">&#9668;</button>
        <button @click="selectNextBases();" class="next-selector">&#9658;</button>
      </div>
    </div>
    <div>
      <h1>Cart</h1>
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
import availableParts from '../data/parts';
import createdHookMixin from './created-hook-mixin';

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
      availableParts,
      cart: [],
      selectHeadIndex: 0,
      selectLeftArmIndex: 0,
      selectRightArmIndex: 0,
      selectTorsosIndex: 0,
      selectBasesIndex: 0,
    };
  },
  mixins: [createdHookMixin],
  computed: {
    headBorderStyle() {
      return {
        border: this.selectedRobot.head.onSale ? '3 px solid red' : '3 px solid #aaa',
      };
    },
    selectedRobot() {
      return {
        head: availableParts.heads[this.selectHeadIndex],
        leftArm: availableParts.arms[this.selectLeftArmIndex],
        torsos: availableParts.torsos[this.selectTorsosIndex],
        rightArm: availableParts.arms[this.selectRightArmIndex],
        bases: availableParts.bases[this.selectBasesIndex],
      };
    },
  },
  methods: {
    addToCart() {
      const robot = this.selectedRobot;
      const cost = robot.head.cost
        + robot.bases.cost
        + robot.leftArm.cost
        + robot.rightArm.cost
        + robot.torsos.cost;
      this.cart.push({ ...robot, cost });
    },
    selectNextHead() {
      const size = availableParts.heads.length;
      this.selectHeadIndex = getNextValidIndex(this.selectHeadIndex, size);
    },
    selectPreviousHead() {
      const size = availableParts.heads.length;
      this.selectHeadIndex = getPreviousValidIndex(this.selectHeadIndex, size);
    },
    selectNextLeftArm() {
      const size = availableParts.arms.length;
      this.selectLeftArmIndex = getNextValidIndex(this.selectLeftArmIndex, size);
    },
    selectPreviousLeftArm() {
      const size = availableParts.arms.length;
      this.selectLeftArmIndex = getPreviousValidIndex(this.selectLeftArmIndex, size);
    },
    selectNextRightArm() {
      const size = availableParts.arms.length;
      this.selectRightArmIndex = getNextValidIndex(this.selectRightArmIndex, size);
    },
    selectPreviousRightArm() {
      const size = availableParts.arms.length;
      this.selectRightArmIndex = getPreviousValidIndex(this.selectRightArmIndex, size);
    },
    selectNextTorsos() {
      const size = availableParts.torsos.length;
      this.selectTorsosIndex = getNextValidIndex(this.selectTorsosIndex, size);
    },
    selectPreviousTorsos() {
      const size = availableParts.torsos.length;
      this.selectTorsosIndex = getPreviousValidIndex(this.selectTorsosIndex, size);
    },
    selectNextBases() {
      const size = availableParts.bases.length;
      this.selectBasesIndex = getNextValidIndex(this.selectBasesIndex, size);
    },
    selectPreviousBases() {
      const size = availableParts.bases.length;
      this.selectBasesIndex = getPreviousValidIndex(this.selectBasesIndex, size);
    },

  },
};
</script>

<style lang="scss" scoped>
.part {
  position: relative;
  width:165px;
  height:165px;
  border: 3px solid #aaa;
}
.part {
  img {
    width: 165px;
  }
}
.top-row {
  display:flex;
  justify-content: space-around;
}
.middle-row {
  display:flex;
  justify-content: center;
}
.bottom-row {
  display:flex;
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
  z-index:1;
  top: -3px;
  left: -28px;
  width: 25px;
  height: 171px;
}
.next-selector {
  position: absolute;
  z-index:1;
  top: -3px;
  right: -28px;
  width: 25px;
  height: 171px;
}
.center .prev-selector, .center .next-selector {
  opacity:0.8;
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
.sale{
  color: red;
}
.add-to-cart{
  position: absolute;
  right: 30px;
  width: 220px;
  padding: 3px;
  font-size: 16px;
}
.content{
  position: relative;
}
td, th {
  text-align: left;
  padding: 5px 20px 5px 5px;
}
.cost{
  text-align: right;
}
.sale-border{
  border: 3px solid red;
}
</style>
