
<script>
export default {
  name: "box",
  props: {
    wh: {
      type: Object,
      required: true
    }
  },
  render() {
    return (
          <div>
            {this.wh.value.map((v)=>{
              <p>{v.x[0]}</p>
            })}
          </div>
        )
  },
  created() {
    let xtotal = [],
      x = 0;
    if (this.wh.type != 0 && this.wh.type != 1) {
      return false;
    }
    for (let i = 0; i < this.wh.value.length; i++) {
      for (let j = 0; j < this.wh.value[i].x.length; j++) {
        if (typeof this.wh.value[i].x[j] == "number") {
          x += this.wh.value[i].x[j];
        } else {
          for (let n = 0; n < this.wh.value[i].x[j].value[0].x.length; n++) {
            x += this.wh.value[i].x[j].value[0].x[n];
          }
        }
      }
      xtotal.push(x);
      x = 0;
    }
    console.log(xtotal);
    let flag = xtotal.some(function(value) {
      return value !== xtotal[0];
    });
    if (flag) {
      alert("数据有误");
    }
  },
  methods: {
    del(i2) {
      this.$emit("changevalue", i2);
    }
  }
};
</script>

<style scoped>
div {
  box-sizing: content-box;
}
.border1 {
  border: 1px solid #000;
  box-sizing: border-box;
}
.border2 {
  border: 2px solid #000;
  box-sizing: content-box;
  margin-bottom: 20px;
}
.box-wrap {
  width: 120px;
  height: 120px;
}
.del {
  display: none;
  position: absolute;
  right: 0;
  top: 0;
  font-size: 20px;
  cursor: pointer;
}
.del:hover {
  color: #ff0000;
}
.showdel:hover .del {
  display: inline;
}
</style>