<template>
  <div  class="card container" >

      <div class="w3-padding-16 w3-center">
        <b id="x1">Playfair Cipher Algorithm</b>
      </div>
      <div class="row">
        <div class="col-md-6">
          <form>
            <div class="form-group">
              <label for="exampleInputEmail1">Key </label>
              <input type="text" class="form-control" id="exampleInputEmail1" placeholder="Key" v-model="keyword">
            </div>
            <div class="form-group">
              <label for="exampleInputPassword1">Plain Text </label>
              <input type="text" class="form-control" id="exampleInputPassword1" placeholder="Plaintext" v-model="plaintext_new">
            </div>
            <div class="w3-center">
              <button type="submit" class="btn btn-default" v-on:click="preProcessing"><b>OK</b></button>
            </div>
          </form>
          <div class="w3-padding-32">
            <p><b>Plain  Text : </b>{{plaintext_show}} </p>
            <p><b>Cipher Text : </b>{{output}} </p>
          </div>

        </div>
        <div class="col-md-6">
      <div class="w3-padding-8">
        <table border="2" v-if="Key_adjust" align=center>
          <tr class="w3-center">
            <td :bgcolor="color[0][0]">{{Key_2D[0][0]}}</td>
            <td :bgcolor="color[0][1]">{{Key_2D[0][1]}}</td>
            <td :bgcolor="color[0][2]">{{Key_2D[0][2]}}</td>
            <td :bgcolor="color[0][3]">{{Key_2D[0][3]}}</td>
            <td :bgcolor="color[0][4]">{{Key_2D[0][4]}}</td>
          </tr>
          <tr class="w3-center">
            <td :bgcolor="color[1][0]">{{Key_2D[1][0]}}</td>
            <td :bgcolor="color[1][1]">{{Key_2D[1][1]}}</td>
            <td :bgcolor="color[1][2]">{{Key_2D[1][2]}}</td>
            <td :bgcolor="color[1][3]">{{Key_2D[1][3]}}</td>
            <td :bgcolor="color[1][4]">{{Key_2D[1][4]}}</td>
          </tr>
          <tr class="w3-center">
            <td :bgcolor="color[2][0]">{{Key_2D[2][0]}}</td>
            <td :bgcolor="color[2][1]">{{Key_2D[2][1]}}</td>
            <td :bgcolor="color[2][2]">{{Key_2D[2][2]}}</td>
            <td :bgcolor="color[2][3]">{{Key_2D[2][3]}}</td>
            <td :bgcolor="color[2][4]">{{Key_2D[2][4]}}</td>
          </tr>
          <tr class="w3-center">
            <td :bgcolor="color[3][0]">{{Key_2D[3][0]}}</td>
            <td :bgcolor="color[3][1]">{{Key_2D[3][1]}}</td>
            <td :bgcolor="color[3][2]">{{Key_2D[3][2]}}</td>
            <td :bgcolor="color[3][3]">{{Key_2D[3][3]}}</td>
            <td :bgcolor="color[3][4]">{{Key_2D[3][4]}}</td>
          </tr>
          <tr class="w3-center">
            <td :bgcolor="color[4][0]">{{Key_2D[4][0]}}</td>
            <td :bgcolor="color[4][1]">{{Key_2D[4][1]}}</td>
            <td :bgcolor="color[4][2]">{{Key_2D[4][2]}}</td>
            <td :bgcolor="color[4][3]">{{Key_2D[4][3]}}</td>
            <td :bgcolor="color[4][4]">{{Key_2D[4][4]}}</td>
          </tr>
        </table><br />
      </div>
      </div>
      </div>
      <div class="row">
        <div class="col-md-12">
          <div class="w3-padding-4 w3-center" v-if="Key_adjust">
            <div class="" v-if="letEn">
              <button @click="encrypt(plainDivided_adjust)"  class="btn btn-default"><b>Encrypt</b></button>
            </div>
            <div class="" v-else>
              <a href="https://playfair-37b14.firebaseapp.com/" class="btn btn-default">refresh</a>
            </div>
          </div>
        </div>
      </div>
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
      plaintext_new: '',
      plaintext_show: '',
      output: '',
      count: 0,
      letEn: true,
      color: [
        ['white', 'white', 'white', 'white', 'white'],
        ['white', 'white', 'white', 'white', 'white'],
        ['white', 'white', 'white', 'white', 'white'],
        ['white', 'white', 'white', 'white', 'white'],
        ['white', 'white', 'white', 'white', 'white']
      ]
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
      vm.prePlaintext()
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
      for (var n = 0; n < vm.plainDivided_adjust.length; n++) {
        vm.plaintext_show += vm.plainDivided_adjust[n] + ' '
      }
    },
    encrypt (item) {
      let vm = this
      for (var row = 0; row < 5; row++) {
        for (var col = 0; col < 5; col++) {
          vm.color[row][col] = 'white'
        }
      }
      var pair = item[vm.count]
      var check = pair.split('')
      var position = {
        char1: {
          row: 0,
          column: 0
        },
        char2: {
          row: 0,
          column: 0
        }
      }

      for (var a = 0; a < 2; a++) {
        for (var r = 0; r < vm.Key_2D.length; r++) {
          for (var c = 0; c < vm.Key_2D[r].length; c++) {
            if (check[a] === vm.Key_2D[r][c]) {
              if (a === 0) {
                position.char1.row = r
                position.char1.column = c
                vm.color[r][c] = '#33FF80'
              } else {
                position.char2.row = r
                position.char2.column = c
                vm.color[r][c] = '#33FF80'
              }
            } else if (check[a] === 'J' && vm.Key_2D[r][c] === 'I') {
              if (a === 0) {
                position.char1.row = r
                position.char1.column = c
                vm.color[r][c] = '#33FF80'
              } else {
                position.char2.row = r
                position.char2.column = c
                vm.color[r][c] = '#33FF80'
              }
            }
          }
        }
      }
      for (var j = 0; j < 2; j++) {
        if (position.char1.row !== position.char2.row) {
          if (position.char1.column !== position.char2.column) {
            if (j === 0) {
              check[j] = vm.Key_2D[position.char1.row][position.char2.column]
              vm.color[position.char1.row][position.char2.column] = '#FF3333'
            } else {
              check[j] = vm.Key_2D[position.char2.row][position.char1.column]
              vm.color[position.char2.row][position.char1.column] = '#FF3333'
            }
            vm.output += check[j]
          } else {
            if (j === 0) {
              if (position.char1.row !== 4) {
                check[j] = vm.Key_2D[position.char1.row + 1][position.char1.column]
                vm.color[position.char1.row + 1][position.char1.column] = '#FF3333'
              } else {
                position.char1.row = 0
                check[j] = vm.Key_2D[position.char1.row][position.char1.column]
                vm.color[position.char1.row][position.char1.column] = '#FF3333'
              }
            } else {
              if (position.char2.row !== 4) {
                check[j] = vm.Key_2D[position.char2.row + 1][position.char2.column]
                vm.color[position.char2.row + 1][position.char2.column] = '#FF3333'
              } else {
                position.char2.row = 0
                check[j] = vm.Key_2D[position.char2.row][position.char2.column]
                vm.color[position.char2.row][position.char2.column] = '#FF3333'
              }
            }
            vm.output += check[j]
          }
        } else {
          if (j === 0) {
            if (position.char1.column !== 4) {
              check[j] = vm.Key_2D[position.char1.row][position.char1.column + 1]
              vm.color[position.char1.row][position.char1.column + 1] = '#ff1a1a'
            } else {
              position.char1.column = 0
              check[j] = vm.Key_2D[position.char1.row][position.char1.column]
              vm.color[position.char1.row][position.char1.column] = '#ff1a1a'
            }
          } else {
            if (position.char2.column !== 4) {
              check[j] = vm.Key_2D[position.char2.row][position.char2.column + 1]
              vm.color[position.char2.row][position.char2.column + 1] = '#ff1a1a'
            } else {
              position.char2.column = 0
              check[j] = vm.Key_2D[position.char2.row][position.char2.column]
              vm.color[position.char2.row][position.char2.column] = '#ff1a1a'
            }
          }
          vm.output += check[j]
        }
      }
      vm.output += ' '
      vm.count++
      if (vm.count === item.length) {
        vm.letEn = false
      }
    }
  }
}
</script>
<style>
#app {

  font-family: 'Muli', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  background-color: #BDBDBD;
  margin-top: 60px;

}
.card {
  @import url('https://fonts.googleapis.com/css?family=Quicksand');
  font-family: 'Quicksand', sans-serif;
    /* Add shadows to create the "card" effect */
    font-size: 20px;
    background-color: white;
    box-shadow: 0 4px 8px 0 rgba(0,0,0,0.2);
    transition: 0.3s;
    border-radius: 15px;

}
.container {
    padding: 2px 16px;
}
table {
  table-layout: auto;

  border-collapse: collapse;
  border: 1px solid black;
  width: 100%;
  height: 500px;


}
td {
  text-align: center;
  width: 20%;
  height: 100px;
}
#x1 {

  font-size:30px;
}
</style>
