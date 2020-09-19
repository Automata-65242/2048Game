<template>
  <header>
    <div id="head">
      <div id="index" class="nav triangle ">
        <svg class="icon" aria-hidden="true" @click="">
          <use xlink:href="#icon-fenlei"></use>
        </svg>导航
        <div id="index_data" class="nav_data">暂无数据</div>
      </div>

      <div id="search" class="nav">
        <input type="text" id="search_input" placeholder="请输入需要搜索的游戏名" />
        <svg class="icon" aria-hidden="true" @click="">
          <use xlink:href="#icon-sousuo"></use>
        </svg>
      </div>



      <div id="tools" class="nav">
        <div class="nav triangle" id="history">
          <svg class="icon" aria-hidden="true">
            <use xlink:href="#icon-lishi"></use>
          </svg>历史记录
          <div id="history_data" class="nav_data">暂无数据</div>
        </div>
        <div class="nav triangle" id="lovely">
          <svg class="icon" aria-hidden="true">
            <use xlink:href="#icon-aixin"></use>
          </svg>猜你喜欢
          <div id="lovely_data" class="nav_data">暂无数据</div>
        </div>
      </div>


    </div>
  </header>
</template>
<script>
  export default {
    // 未使用import导入字体图标,直接在index中引入js

    name: 'VueHead',
    data() {
      return {

      }
    }
  }
</script>


<style>
  .icon {
    width: 1em;
    height: 1em;
    vertical-align: -0.15em;
    fill: currentColor;
    overflow: hidden;
  }

  header {
    background-color: white;
    position: relative;
    height: 2rem;
    width: 100%;
    line-height: 2rem;
    border-bottom: solid 1px #E0E0E0;
    z-index: 1;
    /* overflow: hidden; */
  }

  #head {
    position: absolute;
    left: 25%;
  }

  .nav {
    display: inline-block;
  }


  #search {
    left: 30%;
    height: 30px;
    border: solid 1px gray;
  }

  #search_input {
    /* background-color: #0086B3; */
    width: 300px;
    height: 22px;
    border: 0;
    padding: 0;
    outline-style: none;
  }

  .triangle::after{
    content: '';
    display: inline-block;
    position: relative;
    top: 5px;
    width: 0px;
    height: 0px;
    border-top: 7px solid gray;
    border-left: 7px solid transparent;
    border-right:7px solid transparent ;
    border-bottom: 7px solid transparent;
    /* background-color: #000000; */
  }

  #tools::before {
    content: '';
    display: inline-block;
    width: 150px;
  }

  #index:hover #index_data{
    /* display: block; */
    height: 300px;

  }
  #index:hover{
    background-color: #EEEEEE;
  }

  #history:hover #history_data{
    height: 300px;

  }
  #history:hover{
    background-color: #EEEEEE;
  }

  #lovely:hover #lovely_data{
    height: 300px;
  }
  #lovely:hover{
    background-color: #EEEEEE;
  }

  .nav_data{
    /* display: none; */
    position: absolute;
    background-color: #EEEEEE;
    width: 200px;
    height: 0px;
    padding: solid 1px gray ;
    overflow: hidden;
    transition: 200ms;
  }




</style>
