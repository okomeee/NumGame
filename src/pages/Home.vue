<template>
  <v-ons-page>
    <div class="center">
      <p>N枚のカードを並べます</p>
      <p>生成される数列を当ててください</p>
      <p>同じ数字は使いません</p>
      <span>N: </span>
      <input class="digit" type="text" v-model="digit" placeholder="" @change="checkDig()">
      <span class="digit">枚</span>
      <p>{{ range }}</p>
      <p>半角数字で入力してください</p>
      <div v-if="ans!==inn">
        <h1>{{ num }}</h1>
      </div>
      <div v-if="ans!==''&&ans===inn">
        <h1 class="correct">{{ ans }}</h1>
      </div>
      <div v-if="digit!==''&&ans!==''">
        <span>答え: </span><input type="text" v-model="inn" placeholder="" :style="AnsStyle"  @change="checkIn()">
      </div>
      <div v-if="ans!==''&&ans===inn">
        <h1 class="correct">正解！</h1>
        <v-ons-button modifier="quiet" @click="setAns">リセット</v-ons-button>
      </div>
      <div v-if="hintlists.length !== 0" style="width: 80%; margin: auto;">
        <h2>{{ cnt }}回チャレンンジ済み</h2>
        <v-ons-list modifier="inset">
          <v-ons-list-header>チャレンジ記録</v-ons-list-header>
          <v-ons-list-item v-for="(hint, key, index) in hintlists" :key="index">
            [{{ key + 1 }}]&nbsp;&nbsp;IN:&nbsp;{{ hint.in }}&nbsp;&nbsp;=>&nbsp;&nbsp;<span style="color: blue;">HIT:&nbsp;{{ hint.hit }}&nbsp;&nbsp;BLOW:&nbsp;{{ hint.blow }}</span>&nbsp;&nbsp;=>&nbsp;&nbsp;RESULT:&nbsp;{{ hint.result }}
          </v-ons-list-item>
        </v-ons-list>
      </div>
    </div>
  </v-ons-page>
</template>

<script>
export default {
  name: 'home',
  data () {
    return {
      range: '( 0 < N < 10)',
      digit: '',
      inn: '',
      ans: '',
      cnt: 0,
      hintlists: [],
      hint: {
        in: 0,
        hit: 0,
        blow: 0,
        result: ''
      },
      AnsStyle: {
        'font-size': '18px',
        'margin': '2% auto auto',
        'width': '2em'
      }
    }
  },
  computed: {
    num: () => {
      return '?'.repeat(Number(this.digit))
    }
  },
  watch: {
    cnt: () => {

    }
  },
  methods: {
    checkDig () {
      if (this.digit !== '') {
        if (!isNaN(this.digit)) {
          if (Number(this.digit) < 11 && Number(this.digit) >= 0) {
            this.AnsStyle = {
              'font-size': '18px',
              'margin': '2% auto auto',
              'width': String(Number(this.digit) + 1) + 'em'
            }
            this.setAns()
          } else {
            alert(this.range)
            this.digit = ''
          }
        } else {
          alert('数字を入力してください')
          this.digit = ''
        }
      }
    },
    checkIn () {
      if (!isNaN(this.inn)) {
        if (this.inn.length > Number(this.digit) || this.inn < 0) {
          alert('入力された数列が長すぎます')
          this.inn = ''
        }
      } else {
        alert('数字を入力してください')
        this.inn = ''
      }
      if (this.inn.length === this.ans.length) {
        this.cnt += 1
        this.checkAns()
      }
    },
    checkAns () {
      var innArray = this.inn.split('')
      var ansArray = this.ans.split('')
      var h = 0
      var b = 0
      var r = '不正解'
      for (var i = 0; i < Number(this.digit); i++) {
        if (innArray[i] === ansArray[i]) {
          h += 1
        }
        if (ansArray.indexOf(innArray[i]) >= 0) {
          b += 1
        }
      }
      if (this.ans !== '' && this.ans === this.inn) {
        r = '正解'
      }
      this.hint = {
        in: this.inn,
        hit: h,
        blow: b,
        result: r
      }
      this.hintlists.push(this.hint)
    },
    setAns () {
      this.hintlists = []
      this.cnt = 0
      this.inn = ''
      this.ans = ''
      var r = ''
      var li = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
      while (li.length > (10 - Number(this.digit))) {
        r = Math.floor(Math.random() * li.length)
        this.ans += String(li[r])
        li.splice(r, 1)
      }
      console.log('答えだよ:' + this.ans)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
p {
  margin: 3px auto;
}

.digit {
  font-size: 18px;
  margin: 2% auto auto;
  width: 2em;
}

.button--quiet {
  padding:0 0 1% 1%;
}

.center {
  margin: 5% auto auto;
  text-align: center;
}

.digit {
  font-size: 18px;
}

.correct {
  color: red;
}
</style>
