<template>
  <div class="box" id="demo">
    <ul class="puzzle-wrap">
      <li class="puzzle" :class="{'puzzle-empty':!puzzle}" v-for="(puzzle,index) in puzzles"  @click="moveFn(puzzle,index)">
        <img :src=" 'static/images/'+puzzle+'.png' " :alt="puzzle">
      </li>
    </ul>
    <button class="btn btn-warning btn-block btn-reset" @click = "render">重新开始</button>
  </div>
</template>

<script>
export default {

  //12,9,8,4,5,10,15,7,3,2,13,'',6,1,14,11
  name: 'demo',
  data () {
    return {
      puzzles:[]
    }
  },
  methods:{
    render(){
      let puzzleArr = [];
      for (let i = 1; i < 16; i++) {
        puzzleArr.push(i)//生成1-15数字的数组
      }
      puzzleArr = puzzleArr.sort(()=>{
        return Math.random()-0.5//随机打算数组,这里利用Math.random()生成一个 0 ~ 1 之间的随机数，再减去0.5，这样就会有一半概率返回一个小于 0 的值，一半概率返回一个大于0的值，就保证了生成数组的随机性，实现了动态随机生成数字格子的功能。
      })
      this.puzzles = puzzleArr
      this.puzzles.push('')//空白格子
      console.log(this.puzzles)
    },

    moveFn(puzzle,index){

      let [curNum,leftNum,rightNum,topNum,bottomNum] = [this.puzzles[index],this.puzzles[index-1],this.puzzles[index+1],this.puzzles[index-4],this.puzzles[index+4]];

      console.log(index);

      console.log([curNum,leftNum,rightNum,topNum,bottomNum]);

      if (leftNum === '' && index !== 4 && index !== 8 && index !== 12) {
        this.$set(this.puzzles,index - 1, curNum);
        this.$set(this.puzzles,index, '');
      } else if (rightNum === '' && index !== 3 && index !== 7 && index !== 11) {
        this.$set(this.puzzles,index + 1, curNum);
        this.$set(this.puzzles,index, '');
      } else if (topNum === '') {
        this.$set(this.puzzles,index - 4, curNum);
        this.$set(this.puzzles,index, '');
      } else if (bottomNum === '') {
        this.$set(this.puzzles,index + 4, curNum);
        this.$set(this.puzzles,index, '');
      }
      console.log(this.puzzles);

      this.passFn()
    },
    //检验是否过关
    passFn(){
      if (this.puzzles[15]==='') {//当数组最后一个元素为空时，截取数组的前15个元素生成一个新的数组，通过every方法循环截取后数组的每一个元素是否等于其index+1，array.every(callback[,thisObject]);callback:函数用来测试每个元素thisObject:对象作为该执行回调时使用
        const newPuzzels = this.puzzles.slice(0,15)
        const isPass = newPuzzels.every((e,i) => e === i+1)
        if (isPass) {
          alert('恭喜，你已经成功的拼出了魅力男人！')
        }
      }
    }
  },
  created(){
    this.render()
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="stylus" rel="stylesheet/stylus">
  body
    font-family Arial, "Microsoft YaHei"
    color #fff
  li
    list-style-type none
  .box
    width 400px
    margin 50px auto 0
  .puzzle-wrap
    width 410px
    height 410px
    margin-bottom 40px
    padding 0
    background #ccc
    list-style none
  .puzzle
    float left
    width 100px
    height 100px
    font-size 20px
    background #37c
    text-align center
    line-height 100px
    border: 1px solid #ccc
    box-shadow 1px 1px 4px
    cursor pointer
  .puzzle-empty
    background #ccc
    box-shadow inset 2px 2px 18px
    img
      display none
  .btn-reset
    width 410px
    height 40px
    background #37c
    border none
    color #fff
    outline none
</style>
