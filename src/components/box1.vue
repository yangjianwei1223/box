<template>
    <div class="border2 box-wrap">
        <template v-if="wh.type == 0">
            <template class="border1" v-for="(item, index) in wh.value">
                <template v-for="(i1, index1) in item.x">
                    <div v-if="typeof i1 == 'number'" :key="index + '-' + index1" class="border1 showdel" :style="{width: i1 + 'px', height: item.y + 'px'}" style="float: left;position: relative;">
                        <span class="del" @click="del(index + '-' + index1)">X</span>
                    </div>
                    <template v-else-if="i1.type == 0">
                        <template v-for="(i2, index2) in i1.value">
                            <template v-for="(i3, index3) in i2.x">
                                <div :key="index + '-' + index1 + '-' + index2 + '-' + index3" class="border1 showdel" :style="{width: i3 + 'px', height: i2.y + 'px'}" style="float: left;position: relative;">
                                    <span class="del" @click="del(index + '-' + index1 + '-' + index2 + '-' + index3)">X</span>
                                </div>
                            </template>
                        </template>
                    </template>
                    
                    <div v-else :key="index + 'to' + index1" class="border1 showdel" :style="{width: i1 + 'px', height: item.y + 'px'}" style="float: left;position: relative;">

                    </div>
                </template>
            </template>
        </template>
        <template v-else-if="wh.type == 1">
            <div  v-for="(item, index) in wh.value" :key="index" :style="{width: item.y + 'px', float: 'left'}">
                <div v-for="(i1, index1) in item.x" :key="index + '-' + index1" :style="{width: item.y + 'px', height: i1 + 'px', position: 'relative'}" class="border1 showdel">
                    <span class="del" @click="del(index + '-' + index1)">X</span>
                </div>
            </div>
        </template>
    </div>
</template>

<script>
    export default {
        name: 'box',
        props:{
            wh: {
                type: Object,
                required: true
            }
        },
        created() {
            let xtotal = [], x = 0;
            if (this.wh.type != 0 && this.wh.type != 1) {
                return false;
            }
            for (let i = 0; i < this.wh.value.length; i++) {
                for (let j = 0; j < this.wh.value[i].x.length; j++) {
                    if (typeof this.wh.value[i].x[j] == 'number') {
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
            let flag = xtotal.some(function (value) {
              return value !== xtotal[0];
            });
            if (flag) {
                alert('数据有误');
            }
        },
        methods: {
            del(i2) {
                this.$emit('changevalue', i2);
            }
        },  
    }
</script>

<style scoped>
div{
    box-sizing:content-box;
}
.border1{
    border: 1px solid #000;
    box-sizing:border-box;
}
.border2{
    border: 2px solid #000;
    box-sizing:content-box;
    margin-bottom: 20px;
}
.box-wrap{
    width: 120px;
    height: 120px;
}
.del{
    display: none;
    position: absolute;
    right: 0;
    top: 0;
    font-size: 20px;
    cursor: pointer;
}
.del:hover{
    color:#ff0000;
}
.showdel:hover .del{
    display: inline;
}
</style>