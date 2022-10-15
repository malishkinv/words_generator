<template>
  <div>
    <div class="form">
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
        <div class="col">
          <button @click="generate()">Сгенерировать</button>
        </div>
      </div>
      <div class="row">
        <div class="col">{{ generated.map((word) => word.word).join(', ') }}</div>
      </div>
    </div>
    <div
        class="result"
        :style="{'grid-template-columns': `repeat(${colsCount}, 100px)`, 'grid-template-rows': `repeat(${rowsCount}, 100px)`}"
    >
      <template v-for="(word, wIdx) in generated">
        <template v-if="wIdx < (colsCount * rowsCount)">
          <div class="letter__wrapper" :key="`word${wIdx}`">
            <div
              v-for="(letter, lIdx) in word.word"
              :class="`letter letter${lIdx+1} letter${lIdx+1}--${word.positions[lIdx]}`"
              :key="`letter${lIdx}`"
            >
              {{ letter.toUpperCase() }}
            </div>
          </div>
        </template>
      </template>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      threeLettersWords: "яма, дом, кон, ром, лак, лор, вол, воз, ток, кот, лот, год, мак, бак, рак, мор, три, бра, топ, час, все, пот, мир, кал, юла, ель, ива, око, зло, миг, мел, лов, зов, мат, лад, жор, бит, акт, кол, рот, рок, рог, еда, боб, бор, лес, раб",
      fourLettersWords: "авто, агат, ажур, аист, айва, алло, барс, бард, баня, бант, банк, балл, блиц, бобр, бюро, бюст, быль, было, буян, елей, если, едок, едва, енот, елка, желе, жнец, жезл, жижа, гипс, груз, гнет, гусь, гарь, вилы, воля, весь, ишак, изба, инок, ибис, икры, круг, кофе, ковш, клей, лето, ложа, лыко, наст, ночь, нива",
      fiveLettersWords: "автор, анонс, архив, адрес, астра, булка, багор, баран, батон, вилла, вкруг, вовсю, волхв, гомон, греза, грязь, гамма, дымка, далее, десна, ездок, егерь, ехать, езжай, желчь, жабры, жизнь, жучок, изгиб, кивок, клерк, ковер, копия, крыша, лампа, легат, масло, месса, наказ, недуг, отруб, охота, обгон",
      rowsCount: 2,
      colsCount: 2,
      lettersCount: 3,
      positions: ['left', 'top', 'bottom', 'center', 'right'],
      generated: []
    }
  },
  methods: {
    currentWords() {
      let words = ''
      switch (this.lettersCount) {
        case 4:
          words = this.fourLettersWords.split(',')
          break;
        case 5:
          words = this.fiveLettersWords.split(',')
          break;
        default:
          words = this.threeLettersWords.split(',')
      }
      return words.map((word) => {
        let currentWord = ''
        let currentLetterPositions = []
        let currentLetterPosition = ''
        const _word = word.replace(' ', '')
        return {
          word: _word,
          positions: _word.split('').map(() => {
            if (currentWord !== _word) {
              currentWord = _word
              currentLetterPositions = [...this.positions]
            } else {
              const curPosIndex = currentLetterPositions
                  .findIndex((item) => item === currentLetterPosition)
              currentLetterPositions.splice(curPosIndex, 1)
            }
            currentLetterPosition = currentLetterPositions[this.randomIntFromInterval(0, currentLetterPositions.length - 1)]
            return currentLetterPosition
          })
        }
      });
    },
    randomIntFromInterval(min, max) {
      return Math.floor(Math.random() * (max - min + 1) + min)
    },
    generate() {
      this.generated = this.currentWords()
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
  position: relative;
  width: 80px;
  height: 80px;
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
  width: 80px;
  height: 80px;
  font-size: 96px;
  font-weight: 100;
}
.letter2 {
  width: 50px;
  height: 50px;
  font-size: 66px;
  font-weight: 100;
}
.letter2--bottom {
  bottom: 5px;
  left: 15px;
}
.letter2--top {
  top: 5px;
  left: 15px;
}
.letter2--left {
  left: 5px;
  top: 15px;
}
.letter2--right {
  right: 5px;
  top: 15px
}
.letter2--center {
  left: 15px;
  top: 15px;
}
.letter3 {
  width: 30px;
  height: 30px;
  font-size: 40px;
  font-weight: 100;
}
.letter3--bottom {
  bottom:5px;
  left: 15px;
}
.letter3--top {
  top: 5px;
  left: 15px;
}
.letter3--left {
  top: 15px;
  left: 5px;
}
.letter3--right {
  right: 5px;
  top: 15px;
}
.letter3--center {
  top: 25px;
  left: 25px;
}
.letter4 {
  width: 18px;
  height: 18px;
  font-size: 24px;
  font-weight: 100;
}
.letter4--bottom {
  bottom: 10px;
  left: 32px;
}
.letter4--top {
  top: 10px;
  left: 32px;
}
.letter4--left {
  top: 24px;
  left: 5px;
}
.letter4--right {
  top: 24px;
  right: 5px;
}
.letter4--center {
  top: 32px;
  left: 32px;
}
.letter5 {
  width: 10px;
  height: 10px;
  font-size: 16px;
  font-weight: 100;
}
.letter5--bottom {
  bottom: 5px;
  left: 32px;
}
.letter5--top {
  top: 10px;
  left: 28px;
}
.letter5--left {
  bottom: 32px;
  left: 10px;
}
.letter5--right {
  right: 10px;
  top: 32px;
}
.letter5--center {
  right: 32px;
  top: 32px;
}
</style>
