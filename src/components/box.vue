
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
          <div class="border2 box-wrap">
                {this.wh.type == 0 ? (this.wh.value.map((item, index) => {
                    return (
                        item.x.map((i1, index1) => {
                            return (
                                typeof i1 == 'number' ? (
                                 // 这里边是无嵌套的   
                                 this.fristline(i1, item, index, index1)) : (i1.type == 0 ? (
                                  // 这里边嵌套横版的
                                  this.sencondline(i1,index, index1)                                  
                                ) : (
                                    // 横版里边嵌套竖版的
                                    // this.sencondrow(i1, index, index1)
                                    i1.value.map((item2, index2) => {
                                        return (
                                            <div style={{width: item2.y + 'px', float: 'left'}}>
                                                {item2.x.map((ii3, index3) => {
                                                    return (
                                                        <div style={{width: item2.y + 'px', height: ii3 + 'px', position: 'relative'}} class="border1 showdel">
                                                            <span class="del" onClick={() => this.del(index + '-' + index1 + '-' + index2 + '-' + index3)}>X</span>
                                                        </div>
                                                    )
                                                })}
                                            </div>
                                        )
                                    })
                                ))
                            )
                        })
                    )
                })) : (
                    // 这里是竖版的
                    this.wh.value.map((item, index) => {
                        return (
                            <div style={{width: item.y + 'px', float: 'left'}}>
                                {item.x.map((i1, index1) => {
                                    return (
                                        typeof i1 == 'number' ? (
                                            <div style={{width: item.y + 'px', height: i1 + 'px', position: 'relative'}} class="border1 showdel">
                                                <span class="del" onClick={() => this.del(index + '-' + index1)}>X</span>
                                            </div>
                                        ) : (
                                            i1.type === 0 ? (
                                                // 嵌套横版
                                                i1.value.map((i2, index2) => {
                                                    return (
                                                        i2.x.map((i3, index3) => {
                                                            return (
                                                                <div class="border1 showdel" style={{width: i3 + 'px', height: i2.y + 'px',float: 'left', position: 'relative'}}><span class="del" onClick={() => this.del(index + '-' + index1 + '-' + index2 + '-' + index3)}>X</span></div>
                                                            )
                                                        })
                                                    )
                                                })
                                            ) : (
                                                // 嵌套竖版
                                                i1.value.map((i2, index2) => {
                                                    return (
                                                        <div style={{width: i2.y + 'px', float: 'left'}}>
                                                            {i2.x.map((i3, index3) => {
                                                                return (
                                                                    <div style={{width: i2.y + 'px', height: i3 + 'px', position: 'relative'}} class="border1 showdel">
                                                                        <span class="del" onClick={() => this.del(index + '-' + index1 + '-' + index2 + '-' + index3)}>X</span>
                                                                    </div>
                                                                )
                                                            })}
                                                        </div>
                                                    )
                                                })
                                            )
                                        )
                                        
                                    )
                                })}
                            </div>
                        )
                    })
                )}
          </div>
        )
  },
  created() {
      // 待优化
    let xtotal = [],
      x = 0;
    if (this.wh.type != 0 && this.wh.type != 1) {
      return false;
    }
    if (this.wh.type === 0) {
        // 横版计算宽度相等
        for (let i = 0; i < this.wh.value.length; i++) {
            for (let j = 0; j < this.wh.value[i].x.length; j++) {
                if (typeof this.wh.value[i].x[j] == "number") {
                    x += this.wh.value[i].x[j];
                } else {
                    if (this.wh.value[i].x[j].value[0].type === 0) {
                        for (let n = 0; n < this.wh.value[i].x[j].value[0].x.length; n++) {
                            x += this.wh.value[i].x[j].value[0].x[n];
                        }
                    } else {
                        for (let n = 0; n < this.wh.value[i].x[j].value.length; n++) {
                            x += this.wh.value[i].x[j].value[n].y;
                        }                        
                    }
                }
            }
            xtotal.push(x);
            x = 0;
        }
    } else {
        // 竖版计算高度相等
        for (let i = 0; i < this.wh.value.length; i++) {
            for (let j = 0; j < this.wh.value[i].x.length; j++) {
                if (typeof this.wh.value[i].x[j] == "number") {
                    x += this.wh.value[i].x[j];
                } else {
                    // 竖版里边是横版的
                    if (this.wh.value[i].x[j].type === 0) {                        
                        for (let n = 0; n < this.wh.value[i].x[j].value.length; n++) {
                            x += this.wh.value[i].x[j].value[n].y;
                        }
                    } else {
                        for (let n = 0; n < this.wh.value[i].x[j].value[0].x.length; n++) {
                            x += this.wh.value[i].x[j].value[0].x[n];
                        }                        
                    }
                }
            }
            xtotal.push(x);
            x = 0;
        }
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
    },
    fristline(i1, item, index, index1) {
        return (<div class="border1 showdel" style={{width: i1 + 'px', height: item.y + 'px',float: 'left', position: 'relative'}}><span class="del" onClick={() => this.del(index + '-' + index1)}>X</span></div>);
    },
    sencondline(i1, index, index1) {
        return(
            i1.value.map((i2, index2) => {
                                      return (
                                          i2.x.map((i3, index3) => {
                                              return (
                                                  <div class="border1 showdel" style={{width: i3 + 'px', height: i2.y + 'px', float: 'left',position: 'relative'}}>
                                                    <span class="del" onClick={() => this.del(index + '-' + index1 + '-' + index2 + '-' + index3)}>X</span>
                                                </div>
                                              )
                                          })
                                      )
                                  })  
        )
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