<template>
  <div id="app">
    <div class="left">
      <div @click="showbox(item)" class="pore" v-for="(item, index) in boxArr" :key="index">
        <box :wh="item" @changevalue="changev" />
        <div class="mask"></div>
      </div>
    </div>
    <div class="right">
      <div class="bigbox">
        <box v-show="bigv.value" :wh="bigv" @changevalue="changev" />
      </div>
      <button class="save" @click="save">保存</button>
    </div>
    <!-- 弹出框 -->
    <div class="confirm" v-show="showconfirm">
      <div>
        <div class="title">{{msg}}</div>
        <div>
          <button @click="xzs">是</button>
          <button @click="xyx">否</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import box from "./components/box.vue";

export default {
  name: "App",
  data() {
    return {
      bigv: {},
      showconfirm: false,
      msg: "",
      x: 0,
      y: 0,
      x1: 0,
      y1: 0,
      boxArr: [
        {
          type: 0,
          value: [
            { x: [60, 60], y: 60 },
            { x: [60, 60], y: 60 }
          ]
        },
        {
          type: 0,
          value: [
            { x: [40, 40, 40], y: 40 },
            { x: [40, 40, 40], y: 40 },
            { x: [40, 40, 40], y: 40 }
          ]
        },
        // 横版里边嵌套横版的
        {
          type: 0,
          value: [
            { x: [20, 100], y: 60 },
            {
              x: [
                60,
                {
                  type: 0,
                  value: [
                    { x: [30, 30], y: 30 },
                    { x: [60], y: 30 }
                  ]
                }
              ],
              y: 60
            }
          ]
        },
        // 横版里边嵌套竖版的
        {
          type: 0,
          value: [
            { x: [20, 20, 80], y: 60 },
            {
              x: [
                40,
                {
                  type: 1,
                  value: [
                    { x: [10, 50], y: 40 },
                    { x: [60], y: 40 }
                  ]
                }
              ],
              y: 60
            }
          ]
        },
        // 普通竖版
        {
          type: 1,
          value: [
            { x: [20, 20, 80], y: 60 },
            { x: [40, 80], y: 60 }
          ]
        },
        // 竖版里边嵌套横版的
        {
          type: 1,
          value: [
            { x: [20, 20, 80], y: 60 },
            {
              x: [
                40,
                {
                  type: 0,
                  value: [
                    { x: [20, 40], y: 30 },
                    { x: [60], y: 50 }
                  ]
                }
              ],
              y: 60
            }
          ]
        },
        // 竖版里边嵌套竖版的
        {
          type: 1,
          value: [
            { x: [20, 20, 80], y: 60 },
            {
              x: [
                40,
                {
                  type: 1,
                  value: [
                    { x: [30, 50], y: 20 },
                    { x: [80], y: 40 }
                  ]
                }
              ],
              y: 60
            }
          ]
        }
      ]
    };
  },
  methods: {
    changev(i) {
      //   x,y分别是被删除盒子的序列号；
      this.x = parseInt(i.split("-")[0]);
      this.y = parseInt(i.split("-")[1]);
      this.x1 = parseInt(i.split("-")[2]);
      this.y1 = parseInt(i.split("-")[3]);
      //   console.log(x, y);
      if (!this.x1 && this.x1 != 0) {
        let isfirst = this.y == 0 ? true : false; // 是否第一个盒子
        let islast =
          this.bigv.value[this.x].x.length == this.y + 1 ? true : false; // 是否最后一个盒子
        if (this.bigv.value[this.x].x[this.y] == 600) {
          alert("当前不能再进行删除操作");
          return false;
        }
        if (isfirst) {
          if (typeof this.bigv.value[this.x].x[this.y + 1] == "number") {
            this.bigv.value[this.x].x[this.y + 1] += this.bigv.value[this.x].x[
              this.y
            ];
            this.bigv.value[this.x].x.shift();
          } else {
            alert("二级嵌套无法删除");
            return false;
          }
        } else if (islast) {
          if (typeof this.bigv.value[this.x].x[this.y - 1] == "number") {
            this.bigv.value[this.x].x[this.y - 1] += this.bigv.value[this.x].x[
              this.y
            ];
            this.bigv.value[this.x].x.pop();
          } else {
            alert("二级嵌套无法删除");
            return false;
          }
        } else {
          this.msg = this.bigv.type == 0 ? "是否向左合并？" : "是否向上合并？";
          this.showconfirm = true;
        }
      } else {
        // 这里删的是嵌套的二级盒子
        let isfirst = this.y1 == 0 ? true : false; // 是否第一个盒子
        let islast =
          this.bigv.value[this.x].x[this.y].value[this.x1].x.length ==
          this.y1 + 1
            ? true
            : false; // 是否最后一个盒子
        if (this.bigv.value[this.x].x[this.y].value[this.x1].x.length == 1) {
          alert("当前不能再进行删除操作");
          return false;
        }
        if (isfirst) {
          this.bigv.value[this.x].x[this.y].value[this.x1].x[
            this.y1 + 1
          ] += this.bigv.value[this.x].x[this.y].value[this.x1].x[this.y1];
          this.bigv.value[this.x].x[this.y].value[this.x1].x.shift();
        } else if (islast) {
          this.bigv.value[this.x].x[this.y].value[this.x1].x[
            this.y1 - 1
          ] += this.bigv.value[this.x].x[this.y].value[this.x1].x[this.y1];
          this.bigv.value[this.x].x[this.y].value[this.x1].x.pop();
        } else {
          this.msg = this.bigv.type == 0 ? "是否向左合并？" : "是否向上合并？";
          this.showconfirm = true;
        }
      }
    },
    showbox(v) {
      let vv = JSON.parse(JSON.stringify(v));
      for (let i = 0; i < vv.value.length; i++) {
        for (let j = 0; j < vv.value[i].x.length; j++) {
          if (typeof vv.value[i].x[j] == "number") {
            vv.value[i].x[j] = vv.value[i].x[j] * 5;
          } else {
            for (let m = 0; m < vv.value[i].x[j].value.length; m++) {
              vv.value[i].x[j].value[m].y *= 5;
              for (let n = 0; n < vv.value[i].x[j].value[m].x.length; n++) {
                vv.value[i].x[j].value[m].x[n] =
                  vv.value[i].x[j].value[m].x[n] * 5;
              }
            }
          }
        }
        vv.value[i].y = vv.value[i].y * 5;
      }
      this.bigv = JSON.parse(JSON.stringify(vv));
    },
    xzs() {
      this.showconfirm = false;
      this.bigv.value[this.x].x[this.y - 1] += this.bigv.value[this.x].x[
        this.y
      ];
      this.bigv.value[this.x].x.splice(this.y, 1);
    },
    xyx() {
      this.showconfirm = false;
      this.bigv.value[this.x].x[this.y + 1] += this.bigv.value[this.x].x[
        this.y
      ];
      this.bigv.value[this.x].x.splice(this.y, 1);
    },
    save() {
      // 除优化
      let vv = JSON.parse(JSON.stringify(this.bigv));
      for (let i = 0; i < vv.value.length; i++) {
        for (let j = 0; j < vv.value[i].x.length; j++) {
          if (typeof vv.value[i].x[j] == "number") {
            vv.value[i].x[j] = vv.value[i].x[j] / 5;
          } else {
            for (let m = 0; m < vv.value[i].x[j].value.length; m++) {
              vv.value[i].x[j].value[m].y = vv.value[i].x[j].value[m].y / 5;
              for (let n = 0; n < vv.value[i].x[j].value[m].x.length; n++) {
                vv.value[i].x[j].value[m].x[n] =
                  vv.value[i].x[j].value[m].x[n] / 5;
              }
            }
          }
        }
        vv.value[i].y = vv.value[i].y / 5;
      }
      this.boxArr.push(vv);
    }
  },
  components: {
    box
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 20px;
}
.mask {
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
}
.left {
  display: inline-block;
  width: 144px;
  max-height: 800px;
  overflow-x: hidden;
  overflow-y: auto;
}
.right {
  display: inline-block;
  margin-left: 100px;
  width: 700px;
  height: 700px;
  padding: 50px;
}
.pore {
  position: relative;
}
.bigbox .border2 {
  width: 600px;
  height: 600px;
}
.confirm {
  position: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  background: rgba(0, 0, 0, 0.35);
}
.confirm > div {
  position: fixed;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  width: 600px;
  height: 140px;
  background: #fff;
  border-radius: 10px;
}
.confirm .title {
  line-height: 60px;
}
.confirm button {
  background: #ff4396;
  padding: 10px 40px;
  margin: 10px 20px 0;
  text-decoration: none;
  box-shadow: none;
  cursor: pointer;
  outline: none;
  border: 0;
}
.save {
  background: #3385ff;
  padding: 10px 40px;
  margin: 10px 20px 0;
  color: #fff;
  box-shadow: none;
  cursor: pointer;
  outline: none;
  border: 0;
}
</style>
