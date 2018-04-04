<template>
  <div id="selectAll_01">
<li :class="{'allSelectStyle':allSelectActive}" class="li_style" @click="selectAllItem">{{allSelectLab}}</li>
<li v-for="(item,index) in list" :class="{'active':item.active}" 
class="li_style" @click="selectItem(index)">{{item.lab}}</li>
  </div>
</template>
<script>
export default {
  data() {
    return {
      allSelectLab: "全选",
      allSelectActive: false,
      list: []
    };
  },
  mounted() {
    this.init();
  },
  methods: {
    /**
     * 生成模拟数据；
     */
    init() {
      for (let i = 1; i < 11; i++) {
        let object = {};
        if (i < 10) {
          object.lab = "labal0" + i;
        } else {
          object.lab = "labal" + i;
        }
        object.active = false;
        this.list.push(object);
      }
    },
    /**
     * 全选按钮事件；
     * 用loopListActive控制数据中，active的值；
     */
    selectAllItem() {
      this.allSelectActive = !this.allSelectActive;
      if (this.allSelectActive) {
        this.loopListActive(true);
        this.allSelectLab = "全选";
      } else {
        this.loopListActive(false);
        this.allSelectLab = "全不选";
      }
    },
    /**
     * 单选按钮事件；
     * 如果有选中，将全选按钮的状态改变；并改变文字；
     * 如果单选按钮被全部选中，“全选”按钮也变成选中状态；
     */
    selectItem(index) {
      this.list[index].active = !this.list[index].active;
      if (this.selectedItemCount(this.list) != 0) {
        this.allSelectLab = "全不选";
        this.allSelectActive = false;
      }
      if (this.selectedItemCount(this.list) == this.list.length) {
        this.allSelectLab = "全选";
        this.allSelectActive = true;
      }
    },
    /**
     * 循环数据中，active的值；
     */
    loopListActive(boolean) {
      for (let i = 0; i < this.list.length; i++) {
        this.list[i].active = boolean;
      }
    },
    /**
     * 统计数据中，有多少被选中；
     */
    selectedItemCount(array) {
      let count = 0;
      for (let i = 0; i < array.length; i++) {
        if (array[i].active) {
          count++;
        }
      }
      return count;
    }
  }
};
</script>
<style>
#selectAll_01 {
  width: 200px;
  height: 500px;
}
.li_style {
  list-style-type: none;
  width: auto;
  height: 40px;
  background: #efefef;
  margin-top: 20px;
  display: flex;
  justify-content: center;
  align-items: center;
  border: 1px solid #e4e2e2;
  box-shadow: 0px 4px 4px 0px RGBA(48, 48, 77, 0.05);
  cursor: pointer;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
  border-radius: 4px;
}
.allSelectStyle {
  background-color: brown;
  color: #efefef;
}
.active {
  background-color: brown;
  color: #efefef;
}
</style>

