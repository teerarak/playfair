<template>
  <div class="hello">

      Keywords : <input type="text" name="" v-model="keyword">
      <button v-on:click="preProcessing" type="submit" class="btn btn-default">OK</button>
      <h1>{{Key_adjust}}</h1>

      Plaintext : <input type="text" name="" v-model="plaintext_new">
      <button v-on:click="prePlaintext" type="submit" class="btn btn-default">OK</button>
      <h1>{{plainDivided_adjust}}</h1>
    <table border="2" v-if="Key_adjust" align="center">
      <tr>
        <td>{{Key_2D[0][0]}}</td>
        <td>{{Key_2D[0][1]}}</td>
        <td>{{Key_2D[0][2]}}</td>
        <td>{{Key_2D[0][3]}}</td>
        <td>{{Key_2D[0][4]}}</td>
      </tr>
      <tr>
        <td>{{Key_2D[1][0]}}</td>
        <td>{{Key_2D[1][1]}}</td>
        <td>{{Key_2D[1][2]}}</td>
        <td>{{Key_2D[1][3]}}</td>
        <td>{{Key_2D[1][4]}}</td>
      </tr>
      <tr>
        <td>{{Key_2D[2][0]}}</td>
        <td>{{Key_2D[2][1]}}</td>
        <td>{{Key_2D[2][2]}}</td>
        <td>{{Key_2D[2][3]}}</td>
        <td>{{Key_2D[2][4]}}</td>
      </tr>
      <tr>
        <td>{{Key_2D[3][0]}}</td>
        <td>{{Key_2D[3][1]}}</td>
        <td>{{Key_2D[3][2]}}</td>
        <td>{{Key_2D[3][3]}}</td>
        <td>{{Key_2D[3][4]}}</td>
      </tr>
      <tr>
        <td>{{Key_2D[4][0]}}</td>
        <td>{{Key_2D[4][1]}}</td>
        <td>{{Key_2D[4][2]}}</td>
        <td>{{Key_2D[4][3]}}</td>
        <td>{{Key_2D[4][4]}}</td>
      </tr>
    </table>
  </div>
</template>


<script>
export default {
  mounted () {

  },
  name: 'hello',
  data () {
    return {
      keyword: '',
      preProcess: {

      },
      Key_2D: [
        [' ', ' ', ' ', ' ', ' '],
        [' ', ' ', ' ', ' ', ' '],
        [' ', ' ', ' ', ' ', ' '],
        [' ', ' ', ' ', ' ', ' '],
        [' ', ' ', ' ', ' ', ' ']
      ],
      Key_adjust: '',
      plaintext_adjust: '',
      plaintext: '',
      plainDivided: [],
      plainDivided_adjust: [],
      plaintext_new: ''
    }
  },
  methods: {
    preProcessing () {
      let vm = this
      vm.preProcess = vm.keyword.split('')
      var x = 0
      while (x < vm.preProcess.length) {
        vm.keyword = vm.keyword.replace(' ', '')
        x += 1
      }
      vm.keyword = vm.keyword.toUpperCase()
      vm.setKey()
      vm.genKey()
      vm.format()
    },
    setKey () {
      let vm = this
      var start = 0
      var flag = false
      if (vm.keyword.charAt(0) === 'J') {
        vm.Key_adjust += vm.keyword.charAt(1)
        start = 2
      } else {
        vm.Key_adjust += vm.keyword.charAt(0)
        start = 1
      }
      for (var i = start; i < vm.keyword.length; i++) {
        for (var j = 0; j < vm.Key_adjust.length; j++) {
          if (vm.keyword.charAt(i) === vm.Key_adjust.charAt(j)) {
            flag = true
          }
          if (vm.keyword.charAt(i) === 'J') {
            flag = true
          }
        }
        if (flag === false) {
          vm.Key_adjust += vm.keyword.charAt(i)
        }
        flag = false
      }
    },
    genKey () {
      let vm = this
      var flag = true
      var current = ''
      vm.keyword = vm.Key_adjust
      for (var i = 0; i < 26; i++) {
        current = String.fromCharCode(i + 65)
        // console.log(current)
        if (current === 'J') {
          continue
        }
        for (var j = 0; j < vm.keyword.length; j++) {
          if (current === vm.keyword.charAt(j)) {
            console.log('eiei = ' + vm.keyword.charAt(j))
            flag = false
            break
          }
        }
        if (flag) {
          vm.Key_adjust += current
        }
        flag = true
      }
    },
    format () {
      let vm = this
      var a = 0
      for (var i = 0; i < 5; i++) {
        for (var j = 0; j < 5; j++) {
          vm.Key_2D[i][j] = vm.Key_adjust.charAt(a)
          a++
        }
      }
    },
    prePlaintext () {
      let vm = this
      vm.preProcess = vm.plaintext_new.split('')
      var x = 0
      while (x < vm.preProcess.length) {
        vm.plaintext = vm.plaintext_new.replace(' ', '')
        x += 1
      }
      vm.plaintext = vm.plaintext.toUpperCase()
      vm.dividePlain()
    },
    dividePlain () {
      let vm = this
      for (var i = 0; i < vm.plaintext.length; i += 2) {
        vm.plaintext_adjust += (vm.plaintext.charAt(i) + vm.plaintext.charAt(i + 1) + ' ')
      }
      vm.plainDivided = vm.plaintext_adjust.split(' ')
      vm.plainDivided_adjust = vm.plaintext_adjust.split(' ')
      var number = vm.plainDivided.lastIndexOf('')
      vm.plainDivided.splice(number, 1)
      number = vm.plainDivided_adjust.lastIndexOf('')
      vm.plainDivided_adjust.splice(number, 1)
      vm.plaintext_adjust = vm.plainDivided.join('')
      vm.plainDivided_adjust = vm.plaintext_adjust.split('')
      vm.plaintext = ''
      for (var c = 0; c < vm.plainDivided_adjust.length; c += 2) {
        if (vm.plainDivided_adjust[c] === vm.plainDivided_adjust[c + 1]) {
          vm.plainDivided_adjust.splice(c + 1, 1, 'X', vm.plainDivided_adjust[c + 1])
        }
        if (vm.plainDivided_adjust[c + 1] != null) {
          vm.plaintext += vm.plainDivided_adjust[c] + vm.plainDivided_adjust[c + 1] + ' '
        } else {
          vm.plaintext += vm.plainDivided_adjust[c] + ''
        }
      }
      vm.plainDivided_adjust = vm.plaintext.split(' ')
      if (vm.plainDivided_adjust[vm.plainDivided_adjust.length - 1] === '') {
        vm.plainDivided_adjust.splice(vm.plainDivided_adjust.length - 1, 1)
      } else {
        vm.plainDivided_adjust[vm.plainDivided_adjust.length - 1] = vm.plainDivided_adjust[vm.plainDivided_adjust.length - 1] + 'Z'
      }
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
