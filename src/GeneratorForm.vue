<template>
  <div>
    <form class="form">
      <div class="row">
        <div class="col">Кол-во столбцов</div>
        <div class="col">
          <select v-model="colsCount">
            <template v-for="(val, idx) in 10">
              <option :value="val" :key="idx">{{ val }}</option>
            </template>
          </select>
        </div>
      </div>
      <div class="row">
        <div class="col">Кол-во строк</div>
        <div class="col">
          <select v-model="rowsCount">
            <template v-for="(val, idx) in 10">
              <option :value="val" :key="idx">{{ val }}</option>
            </template>
          </select>
        </div>
      </div>
      <div class="row">
        <div class="col">Кол-во букв в словах</div>
        <div class="col">
          <select v-model="lettersCount">
            <template v-for="(val, idx) in [3,4,5]">
              <option :value="val" :key="idx">{{ val }}</option>
            </template>
          </select>
        </div>
      </div>
      <div class="row">
        <div class="col">Слова</div>
      </div>
      <div class="row">
        <div class="col">{{ currentWords.join(', ') }}</div>
      </div>
    </form>
    <div
        class="result"
        :style="{'grid-template-columns': `repeat(${colsCount}, 100px)`, 'grid-template-rows': `repeat(${rowsCount}, 100px)`}"
    >
      <template v-for="(word, wIdx) in currentWords">
        <template v-if="wIdx < colsCount*rowsCount">
          <div class="letter__wrapper" :key="`word${wIdx}`">
            <template v-for="(letter, lIdx) in word">
              <div
                :class="`letter letter${lIdx+1} letter${lIdx+1}--${letterPosition(word)}`"
                :key="`letter${lIdx}`"
              >
                {{ letter.toUpperCase() }}
              </div>
            </template>
          </div>
        </template>
      </template>
    </div>
  </div>
</template>

<script>
const threeLettersWords = "яма, дом, кон, ром, лак, лор, вол, воз, ток, кот, лот, год, мак, бак, рак, мор, три, бра, топ, час, все, пот, мир, кал, юла, ель, ива, око, зло, миг, мел, лов, зов, мат, лад, жор, бит, акт, кол, рот, рок, рог, еда, боб, бор, лес, раб";
const fourLettersWords = "авто, агат, ажур, аист, айва, алло, барс, бард, баня, бант, банк, балл, блиц, бобр, бюро, бюст, быль, было, буян, елей, если, едок, едва, енот, елка, желе, жнец, жезл, жижа, гипс, груз, гнет, гусь, гарь, вилы, воля, весь, ишак, изба, инок, ибис, икры, круг, кофе, ковш, клей, лето, ложа, лыко, наст, ночь, нива";
const fiveLettersWords = "автор, анонс, архив, адрес, астра, булка, багор, баран, батон, вилла, вкруг, вовсю, волхв, гомон, греза, грязь, гамма, дымка, далее, десна, ездок, егерь, ехать, езжай, желчь, жабры, жизнь, жучок, изгиб, кивок, клерк, ковер, копия, крыша, лампа, легат, масло, месса, наказ, недуг, отруб, охота, обгон";

export default {
  data() {
    return {
      rowsCount: 2,
      colsCount: 2,
      lettersCount: 3,
      positions: ['left', 'top', 'bottom', 'center', 'right'],
      currentLetterPosition: '',
      currentWord: ''
    }
  },
  computed: {
    threeLettersWords() {
      return threeLettersWords
          .split(',')
          .map((word) => word.replace(' ', ''))
    },
    fourLettersWords() {
      return fourLettersWords
          .split(',')
          .map((word) => word.replace(' ', ''))
    },
    fiveLettersWords() {
      return fiveLettersWords
          .split(',')
          .map((word) => word.replace(' ', ''))
    },
    currentWords() {
      let words = ''
      switch (this.lettersCount) {
        case 3: words = this.threeLettersWords; break;
        case 4: words = this.fourLettersWords; break;
        case 5: words = this.fiveLettersWords; break;
      }
      return words
    }
  },
  methods: {
    randomIntFromInterval(min, max) {
      return Math.floor(Math.random() * (max - min + 1) + min)
    },
    letterPosition(word) {
      console.log(word)
      let positions = [...this.positions]
      if (this.currentWord !== word) {
        this.currentWord = word
      } else {
        const curPosIndex = this.positions.findIndex((item) => item === this.currentLetterPosition)
        positions.splice(curPosIndex, 1)
      }
      this.currentLetterPosition = positions[this.randomIntFromInterval(0, positions.length - 1)]
      return this.currentLetterPosition
    }
  }
}
</script>
<style>
.form {
  margin: 0 auto;
  width: 500px;
  padding: 24px;
  border: 1px solid #9f9f9f;
  border-radius: 6px;
}
.row {
  display: flex;
  align-items: center;
  margin-bottom: 12px;
}
.row:last-child {
  margin-bottom: 0;
}
.col {
  flex: 1;
  text-align: left;
}
select {

  padding: 4px 6px;
  width: 80px;
}
button {
  border: 1px solid #9f9f9f;
  padding: 6px 10px;
  border-radius: 4px;
}
.result {
  width: fit-content;
  margin: 24px auto auto;
  display: grid;
  grid-column-gap: 20px;
  grid-row-gap: 20px;
}
.letter__wrapper {
  box-sizing: border-box;
  border: 1px solid #aeaeae;
  position: relative;
  width: 100px;
  height: 100px;
}
.letter {
  box-sizing: border-box;
  display: flex;
  align-items: center;
  justify-content: center;
  opacity: 0.8;
  position: absolute;
}
.letter1 {
  background-color: #e5e5e5;
  width: 100px;
  height: 100px;
  font-size: 126px;
  font-weight: 100;
}
.letter2 {
  background-color: #55fae5;
  width: 50px;
  height: 50px;
  font-size: 66px;
  font-weight: 100;
}
.letter2--bottom {
  bottom: 5px;
  left: 25px;
}
.letter2--top {
  top: 5px;
  left: 25px;
}
.letter2--left {
  left: 5px;
  top: 25px;
}
.letter2--right {
  right: 5px;
  top: 25px;
}
.letter2--center {
  left: 25px;
  top: 25px;
}
.letter3 {
  background-color: #ffabff;
  width: 30px;
  height: 30px;
  font-size: 40px;
  font-weight: 100;
}
.letter3--bottom {
  bottom:5px;
  left:35px;
}
.letter3--top {
  top:5px;
  left:35px;
}
.letter3--left {
  top: 35px;
  left: 5px;
}
.letter3--right {
  right: 5px;
  top: 35px;
}
.letter3--center {
  top: 35px;
  left: 35px;
}
.letter4 {
  background-color: #a1cdee;
  width: 18px;
  height: 18px;
  font-size: 24px;
  font-weight: 100;
}
.letter4--bottom {
  bottom:10px;
  left: 41px;
}
.letter4--top {
  top:10px;
  left: 41px;
}
.letter4--left {
  top: 41px;
  left: 5px;
}
.letter4--right {
  top: 41px;
  right: 5px;
}
.letter4--center {
  top: 41px;
  left: 41px;
}
.letter5 {
  background-color: #e2e89c;
  width: 10px;
  height: 10px;
  font-size: 16px;
  font-weight: 100;
}
.letter5--bottom {
  bottom: 5px;
  left: 44px;
}
.letter5--top {
  top: 10px;
  left: 40px;
}
.letter5--left {
  bottom: 44px;
  left: 10px;
}
.letter5--right {
  right: 10px;
  top: 44px;
}
.letter5--center {
  right: 48px;
  top: 48px;
}
</style>
