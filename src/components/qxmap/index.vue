<template>
  <div class="qxmapBox">
    <h1>全国迁徙热门城市</h1>
    <div class="timeBox">
      <pre>{{time}}更新  来自百度地图</pre>
    </div>

    <div>
      <switchtab :list="['热门迁入地','热门迁出地']" @change="change"></switchtab>
    </div>
    <div>
      <mylist :data='list'></mylist>
    </div>
  </div>
</template>
<script>
// @ is an alias to /src
import { moveInOut } from "@/api";
import switchtab from "./switchTable";
import mylist from "./list";
export default {
  name: "qxmap",
  data() {
    return {
      moveInList: [],
      moveOutList: [],
      list:[],
      time:''
    };
  },
  created() {
    moveInOut().then(data => {
      console.log(data);
       this.list=this.moveInList = data.result.moveInList||[];
      this.moveOutList = data.result.moveOutList;
      this.time=data.result.time.replace(/(\d{4})(\d{2})(\d{2})/,'$1年$2月$3日')
      
    //   this.list=this.moveInList;
        //  this.list.myIndex=0;
    });
  },
  components: {
    switchtab,
    mylist
  },
  methods: {
    change(index, item) {
      //   console.log(item);
     
      this.list = index ?  this.moveOutList:this.moveInList ;
       this.list.myIndex=index;
    }
  }
};
</script>
<style lang="less">
.qxmapBox {
  width: 700px;
  margin: 10px auto;
  background: #eee;
  border-radius: 10px;
  h1 {
    text-align: center;
    position: relative;
    height: 50px;
    line-height: 50px;
    &::before,
    &::after {
      content: "";
      display: inline-block;
      width: 8px;
      height: 8px;
      background: #42b983;
      border-radius: 50%;
      //   padding: 5px;
      margin: auto 20px;
    }
  }
  .timeBox {
    text-align: center;
    font-size: 15px;
    color: #ccc;
  }
}
</style>