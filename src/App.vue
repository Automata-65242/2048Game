<template>

  <div id="app">
    <VueHead />

    <div id="game">
      <div id="gamemap" tabindex="0" @keyup="show($event)">
        <div id="pause">点击此处回到游戏</div>
        <!-- 游戏地图 -->
        <div class="mapcell adrx0 adry0"></div>
        <div class="mapcell adrx0 adry1"></div>
        <div class="mapcell adrx0 adry2"></div>
        <div class="mapcell adrx0 adry3"></div>

        <div class="mapcell adrx1 adry0"></div>
        <div class="mapcell adrx1 adry1"></div>
        <div class="mapcell adrx1 adry2"></div>
        <div class="mapcell adrx1 adry3"></div>

        <div class="mapcell adrx2 adry0"></div>
        <div class="mapcell adrx2 adry1"></div>
        <div class="mapcell adrx2 adry2"></div>
        <div class="mapcell adrx2 adry3"></div>

        <div class="mapcell adrx3 adry0"></div>
        <div class="mapcell adrx3 adry1"></div>
        <div class="mapcell adrx3 adry2"></div>
        <div class="mapcell adrx3 adry3"></div>


        <!-- 游戏元素 -->
        <!-- 二维数组 -->
        <div v-for="(item,x) in map">
          <div v-for="(items,y) in item" v-if="items!=0" class="mapcell" :class="['adrx'+x+' adry'+y+' clr'+items]"
            :ref="'cell'+x+y">
            {{items}}
          </div>
        </div>

        <!-- 一维数组,使用vue的列表过度，舍弃-->
        <!--  <transition-group name="flip-list" tag="div">
          <div v-for="(item,index) in maplist" :class="'mapcell ads'+index" :key="'index'+index" v-if="item!=0" :ref="index">{{item}}</div>
        </transition-group> -->

      </div>

      <div id="message">
        <span id="explain">游戏说明：<br />使用方向键使所有方块向目标方向移动，数字相同的方块会合并相加
        <br />当方块中的数字达到2048时完成游戏目标</span>
        <br /><br /><br /><br />
        {{"步数："+count+"&nbsp;&nbsp;&nbsp;&nbsp;分数："+score}}
        <br /><br /><br />

        <button @click="restart" class="btn" title="点击此处重新开始游戏!">
          <svg class="icon2" aria-hidden="true">
            <use xlink:href="#icon-restart"></use>
          </svg>
        </button>

        <button @click="end" class="btn" title="点击此处完成游戏!">
          <svg class="icon2" aria-hidden="true">
            <use xlink:href="#icon-jieshu"></use>
          </svg>
        </button>

        <br /><br /><br /><br />

        <button title="点击切换显示" @click="showchange" v-show="!datashow">开启数据监控：</button>
        <button title="点击切换显示" @click="showchange" v-show="datashow">关闭数据监控：</button>


        <transition name="bounce">
          <table v-show="datashow">
            <tr><th>{{map[0][0]}}</th><th>{{map[0][1]}}</th><th>{{map[0][2]}}</th><th>{{map[0][3]}}</th></tr>
            <tr><th>{{map[1][0]}}</th><th>{{map[1][1]}}</th><th>{{map[1][2]}}</th><th>{{map[1][3]}}</th></tr>
            <tr><th>{{map[2][0]}}</th><th>{{map[2][1]}}</th><th>{{map[2][2]}}</th><th>{{map[2][3]}}</th></tr>
            <tr><th>{{map[3][0]}}</th><th>{{map[3][1]}}</th><th>{{map[3][2]}}</th><th>{{map[3][3]}}</th></tr>
          </table>
        </transition>

      </div>
    </div>
  </div>
</template>

<script>
  import VueHead from './components/VueHead'
  import HelloWorld from './components/HelloWorld'

  export default {
    name: 'App',
    data() {
      return {
        datashow:false,
        count: 0,
        score: 0,
        gameend: false,
        gamecontinue: false,
        maplist: [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        maplistdemo: [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15],
        map: [
          [0, 0, 0, 0],
          [0, 0, 0, 0],
          [0, 0, 0, 0],
          [0, 0, 0, 0]
        ],
        mapdemo: [
          [0, 1, 2, 3],
          [4, 5, 6, 7],
          [8, 9, 10, 11],
          [12, 13, 14, 15]
        ]
      }
    },
    methods: {
      show(e) {
        // console.log(e.keyCode); //获取键码
        // if (e.keyCode == 39) this.$options.methods.right();

        //取代this以解决调用方法时this指向改变问题
        // var vm = this;
        // console.log(vm.map === this.map) //判断成功
        // console.log(vm === this.vm)      //避免重复定义指向

        switch (e.keyCode) {
          case 37:
            //警告！此处会改变this指向！使用call()或者传入vm对象，此处call()
            // this.$options.methods.left(this.vm);
            this.$options.methods.left.call(this);
            // this.$options.methods.leftlist.call(this);
            break;
          case 38:
            this.$options.methods.up.call(this);
            break;
          case 39:
            this.$options.methods.right.call(this);
            break;
          case 40:
            this.$options.methods.down.call(this);
            break;
        }
      },

      /*
      leftlist(){

        for(let j=0;j<16;j+=4){
        				 for(let k=j;k<j+4;k++){
                   //若当前元素为0
                   if(this.maplist[k]==0){
                     //向后在当前行寻找非零元素
                     for(let i=k+1;i<j+4;i++){
                       if(this.maplist[i]!=0){
                         // this.$refs[i][0].setAttribute("key",'index'+k);
                         console.log(this.$refs[i][0])
                         this.maplist[k]=this.maplist[i];
                         this.maplist[i]=0;
                         // this.$refs[i][0].setAttribute("class",'mapcell ads'+k);
                         break;
                       }
                     }
                   }else{
                     for(let i=k+1;i<j+4;i++){
                       if(this.maplist[i]==this.maplist[k]){
                         // this.$refs[i][0].setAttribute("class",'mapcell ads'+k);
                         this.maplist[k]*=2;
                         this.maplist[i]=0;
                         break;
                       }
                     }
                   }
        				 }

        }
        this.newtwolist();
        this.count++;

      },
      */




      left() {
        // console.log("left!");
        // console.log(vm.map)  //判断传入是否成功
        let x, y, i;

        for (x = 0; x < 4; x++) {
          //标记元素是否已经合并过
          let flag = [0, 0, 0, 0]
          for (y = 0; y < 4; y++) {
            if (this.map[x][y] == 0) {
              //若当前元素为0，向后寻找第一个非0元素
              for (i = y + 1; i < 4; i++) {
                if (this.map[x][i] != 0) {
                  //将找到的元素置前
                  // this.$refs["cell"+x+i][0].setAttribute("class","mapcell adrx"+x+" adry"+y);
                  this.map[x][y] = this.map[x][i];
                  this.map[x][i] = 0;
                  if (this.map[x][y] == 2048)
                    this.gameend = true;
                  // this.count++;
                  break;
                }
              }
            } else {
              //若当前元素非0，向后寻找第一个等价元素
              for (i = y + 1; i < 4; i++) {
                if (this.map[x][i] == this.map[x][y]) {
                  //将找到的元素置前
                  // this.$refs["cell"+x+i][0].setAttribute("class","mapcell adrx"+x+" adry"+y);
                  this.map[x][y] *= 2;
                  // this.map[x][y]=this.map[x][i]*2;
                  //map[x][i]导致动画bug
                  this.map[x][i] = 0;
                  this.score += this.map[x][y];
                  if (this.map[x][y] == 2048)
                    this.gameend = true;
                  // this.count++;
                  break;
                }
              }
            }
          }
        }



        /* 不合动画逻辑，无动画实现
        for(i = 0;i < 3;i++){
                for(x=3;x>0;x--)
                {
                    for(y=0;y<4;y++)
                    {
                        if(this.map[y][x-1] == 0)
                        {
                            this.map[y][x-1] =  this.map[y][x];
                            this.map[y][x] = 0;
                        }else
                        {
                            if( this.map[y][x-1] ==  this.map[y][x]){
                                this.score+= this.map[y][x];
                                this.map[y][x-1] *= 2;
                                this.map[y][x] = 0;
                            }
                        }
                    }
                }
            } */


        //动画实现版本1
        /*
        当恰巧新产生的元素所在位置之前有元素被移动
        被移动元素的动画效果会嫁接到新产生的元素上
        不影响数据，仅动画和显示效果改变
        例如【0,0,0,2】向左移动后，在第三个位置出现新元素，结果应为【2,0,2,0】
        理想动画效果为：第四个元素移动到第1位，新元素出现在第3个位置
        实际效果为：第四个元素瞬移至第1位，新元素从第4位出现并移动到第3位
        动画效果待后续改进-----已经改进
        */

        /*
        //使用数组find方法，出现动画bug
        for(x=0;x<4;x++){
          //定义这一行的元素是否已经合并过
          let flag=[0,0,0,0]
          for(y=0;y<4;y++){
            // 定义临时下标
            // let tempy=-1;

            //找到每/第一行第一个不为0的元素
            // let tempy = this.map[x].findIndex((value)=>value!=0)
            //找到每一行由y起第一个不为0的元素并返回该元素的值与下标
            // let tempvalue=this.map[x].find((value,index) =>{if(index>=y&&value!=0){tempy = index;return value!=0}})

            //由于向右或向上移动时需要寻找数组末尾的元素，出于代码复用性考虑，此处改为findindex
            let tempvalue = 0;
            let tempy = this.map[x].findIndex((value,index)=>{if(index>=y&&value!=0){tempvalue=value;return index>=y}})

            //若该元素存在，将其移动
            if(tempy != -1){

              //自第2个元素起(下标1)，若其与上一个目的地元素等值，且上一个元素未合并过
               if((y>0) && (this.map[x][tempy]==this.map[x][y-1]) &&(flag[y-1]==0)){
                 //将该元素与上一个元素合并
                  this.$refs["cell"+x+tempy][0].setAttribute("class","mapcell adrx"+x+" adry"+y-1);
                  //修改数据
                   flag[y-1]=1;
                   this.map[x][tempy]=0;
                   this.map[x][y]=0;
                   this.map[x][y-1]=tempvalue*2;
               }else{
                 //将该元素置前，发生动画
                 this.$refs["cell"+x+tempy][0].setAttribute("class","mapcell adrx"+x+" adry"+y);
                 //修改数据
                 setTimeout(function (){
                    console.log("hello!")
                 }, 200);
                 this.map[x][tempy]=0;
                 this.map[x][y]=tempvalue;
               }
            }else{
              //若后续元素皆为0，终止
              break;
            }
          }
        }
        */



        //操作完成，产生新元素
        // this.count++;
        // this.newtwo();

        //延迟以防止动画未完成时新生成的元素挤占动画后的位置
        var vm = this;
        setTimeout(function() {
          vm.newtwo();
        }, 200);
      },


      //矩阵转置后重调用left()出现动画异常，重写代码
      right() {
        // console.log("right!");
        let x, y, i;
        var vm = this; //

        for (x = 0; x < 4; x++) {
          let flag = [0, 0, 0, 0]
          for (y = 3; y >= 0; y--) {
            if (this.map[x][y] == 0) {
              for (i = y - 1; i >= 0; i--) {
                if (this.map[x][i] != 0) {
                  // this.$refs["cell"+x+i][0].setAttribute("class","mapcell adrx"+x+" adry"+y);
                  this.map[x][y] = this.map[x][i];
                  this.map[x][i] = 0;
                  if (this.map[x][y] == 2048)
                    this.gameend = true;
                  break;
                }
              }
            } else {
              //若当前元素非0，向后寻找第一个等价元素
              for (i = y - 1; i >= 0; i--) {
                if (this.map[x][i] == this.map[x][y]) {
                  // this.$refs["cell"+x+i][0].setAttribute("class","mapcell adrx"+x+" adry"+y);
                  this.map[x][y] *= 2;
                  // this.map[x][y]=this.map[x][i]*2;
                  this.map[x][i] = 0;
                  this.score += this.map[x][y];
                  if (this.map[x][y] == 2048)
                    this.gameend = true;
                  // this.count++;
                  break;
                }
              }
            }


          }
        }

        // this.count++;

        var vm = this;
        setTimeout(function() {
          vm.newtwo();
        }, 200);
      },


      up() {
        // console.log("up!");
        let x, y, i;

        for (y = 0; y < 4; y++) {
          let flag = [0, 0, 0, 0];

          for (x = 0; x < 4; x++) {
            if (this.map[x][y] == 0) {
              for (i = x + 1; i < 4; i++) {
                if (this.map[i][y] != 0) {
                  // this.$refs["cell"+i+y][0].setAttribute("class","mapcell adrx"+x+" adry"+y);
                  this.map[x][y] = this.map[i][y];
                  this.map[i][y] = 0;
                  if (this.map[x][y] == 2048)
                    this.gameend = true;
                  // this.count++;
                  break;
                }
              }
            } else {
              for (i = x + 1; i < 4; i++) {
                if (this.map[i][y] == this.map[x][y]) {
                  this.map[x][y] *= 2;
                  this.map[i][y] = 0;
                  this.score += this.map[x][y];
                  if (this.map[x][y] == 2048)
                    this.gameend = true;
                  // this.count++;
                  break;
                }
              }
            }
          }
        }

        var vm = this;
        setTimeout(function() {
          vm.newtwo();
        }, 200);
      },


      down() {
        // console.log("down!");
        let x, y, i;

        for (y = 0; y < 4; y++) {
          let flag = [0, 0, 0, 0]
          for (x = 3; x >= 0; x--) {
            if (this.map[x][y] == 0) {
              for (i = x - 1; i >= 0; i--) {
                if (this.map[i][y] != 0) {
                  // this.$refs["cell"+i+y][0].setAttribute("class","mapcell adrx"+x+" adry"+y);
                  this.map[x][y] = this.map[i][y];
                  this.map[i][y] = 0;
                  if (this.map[x][y] == 2048)
                    this.gameend = true;
                  // this.count++;
                  break;
                }
              }
            } else {
              for (i = x - 1; i >= 0; i--) {
                if (this.map[i][y] == this.map[x][y]) {
                  this.map[x][y] *= 2;
                  this.map[i][y] = 0;
                  this.score += this.map[x][y];
                  if (this.map[x][y] == 2048)
                    this.gameend = true;
                  break;
                }
              }
            }
          }
        }

        var vm = this;
        setTimeout(function() {
          vm.newtwo();
        }, 200);

      },



      newtwo() {

        //在添加新元素前刷新界面
        // let a,b;
        // for(a=0;a<3;a++){
        //   for(b=0;b<3;b++){
        //     if(this.map[a][b]!=0){
        //     // this.$refs["cell"+a+b][0].setAttribute("class","mapcell adrx"+a+" adry"+b);
        //     this.count++;
        //     }
        //   }
        // }


        //防止继续游戏后再次2048时重复提醒
        if (this.gameend == true && this.gamecontinue == false) {
          this.gamecontinue = true;
          alert("目标达成！点击确认继续游戏");
        }


        let x, y, flag = 0;

        //遍历数组
        for (let e of this.map) {
          if (flag == 1) break;
          for (let f of e) {
            if (f == 0) {
              flag = 1;
              // console.log(flag);
              break;
            }
          }
        }

        while (1) {
          //判断是否有空位生成新元素
          if (flag == 0) {
            console.log("break!");
            break;
          }

          //随机寻位
          x = Math.floor(Math.random() * 4);
          y = Math.floor(Math.random() * 4);
          // console.log(x,y);

          //创建新元素
          if (this.map[x][y] == 0) {
            this.map[x][y] = 2;
            this.count++;
            break;
          }
        }
      },

      newtwolist() {
        while ((this.maplist.findIndex((value) => value == 0)) != -1) {
          let i = Math.floor(Math.random() * 16);
          if (this.maplist[i] == 0) {
            this.maplist[i] = 2;
            this.count++;
            break;
          }
        }

      },

      restart() {
        this.map = [
          [0, 0, 0, 0],
          [0, 0, 0, 0],
          [0, 0, 0, 0],
          [0, 0, 0, 0]
        ];
        this.count = 0;
        this.score = 0;
        this.gameend = false;
        this.gamecontinue = false;
        this.newtwo();
        this.count--;
      },

      end() {
        this.map[3][0] = 1024;
        this.map[3][1] = 1024;
        this.count++;
      },

      showchange(){
        this.datashow = !this.datashow;
      },

      text() {
        // console.log(this.$refs.cell22[0])
        // let tx = 0,ty=3;
        // //使用了v-for，使用ref返回值需【0】
        // this.$refs.cell33[0].setAttribute("class", "mapcell adrx"+tx+" adry"+ty);
        // this.map[3][3]=0;
        // this.$refs.cell33[0].setAttribute("ref","cell03")
        // this.$refs.cell33[1].setAttribute("ref","cell03")
      }

    },
    components: {
      HelloWorld,
      VueHead
    }
  }
</script>

<style>
  html,body,div,span,td,tr {
    margin: 0;
    padding: 0;
  }

  .icon2 {
    width: 3em;
    height: 3em;
    vertical-align: -0.15em;
    fill: currentColor;
    overflow: hidden;
  }

  .bounce-enter-active {
    animation: bounce-in .5s;
  }
  .bounce-leave-active {
    animation: bounce-in .5s reverse;
  }
  @keyframes bounce-in {
    0% {
      transform: scale(0);
    }
    50% {
      transform: scale(1.5);
    }
    100% {
      transform: scale(1);
    }
  }

  div:focus {
    outline: blue solid 2px;
  }

  #game {
    position: absolute;
    width: 1200px;
    height: 800px;
    /* background-color: #42B983; */
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    background-image: url(../static/background.jpg);
    background-size: 100% 100%;
  }

  #gamemap {
    position: absolute;
    width: 500px;
    height: 500px;
    background-color: #bbada0;
    top: 50%;
    left: 25%;
    transform: translate(-50%, -50%);
    border-radius: 20px;
  }

  .mapcell {
    position: absolute;
    width: 100px;
    height: 100px;
    background-color: #ccc0b3;
    margin: 20px 0 0 20px;
    border-radius: 10px;
    transition: 200ms;
    z-index: -1;
    text-align: center;
    line-height: 100px;
    font-size: 3rem;
  }

  #message {
    position: absolute;
    /* background-color: #61C0ED; */
    width: 360px;
    height: 460px;
    border: solid 1px grey;
    padding: 20px;
    top: 50%;
    left: 70%;
    transform: translate(-35%, -50%);
  }

  #explain{
    color: #F65E3B;
    opacity: .8;
  }

  #pause{
    width: 100%;
    height: 100%;
    background: rgba(0,0,0, .4);
    transition: 500ms;
    line-height: 500px;
    text-align: center;
    font-size: 40px;
  }

  div:focus #pause {
    /* z-index: -2; */
    opacity: 0;
    visibility: hidden;
  }

  th {
    width: 20px;
  }

  .btn {
    background: transparent;
    border: 0;
    outline: none;
    /* outline: solid 1px gray; */
  }


  /* 定义横坐标 */
  .adrx0 {top: 0px;}
  .adrx1 {top: 120px;}
  .adrx2 {top: 240px;}
  .adrx3 {top: 360px;}

  /* 定义纵坐标 */
  .adry0 {left: 0px;}
  .adry1 {left: 120px;}
  .adry2 {left: 240px;}
  .adry3 {left: 360px;}

  /* 定义元素颜色 */
  .clr2 {background-color: #f8f3e8;}
  .clr4 {background-color: #ede0c8;}
  .clr8 {color: #f9f6f2;    background: #f2b179;}
  .clr16 {color: #f9f6f2;   background: #f59563;}
  .clr32 {color: #f9f6f2;   background: #f67c5f;}
  .clr64 {color: #f9f6f2;   background: #f65e3b;}
  .clr128 {color: #f9f6f2;  background: #edcf72;}
  .clr256 {color: #f9f6f2;  background: #edcc61;}
  .clr512 {color: #f9f6f2;  background: #edc850;}
  .clr1024 {color: #f9f6f2; background: #edc53f;  font-size: 2.6rem;}
  .clr2048 {color: #f9f6f2; background: #edc22e;  font-size: 2.6rem;}
  .clr4096 {color: #f9f6f2; background: #61c0ed;  font-size: 2.6rem;}


</style>
