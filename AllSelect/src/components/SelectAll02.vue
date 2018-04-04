<template>
  <div id="selectAll_02">
<li :class="{'allSelectStyle':allSelectActive}" class="li_style" @click="selectAllItem">{{allSelectLab}}</li>
<li v-for="(item,index) in list" :class="{'active':item.active}" 
class="li_style" @click="selectItem(index)">{{item.lab}}</li>
  </div>
</template>
<script>
export default {
  data() {
    return {
      allSelectLab: "至少选一项",
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
     * 如果选中的数据小于数据长度，将所有单选按钮设置成选中状态；
     * 如果所以单选按钮被选中，将取消全部选中状态；
     */
    selectAllItem() {
      this.allSelectActive = !this.allSelectActive;
      if (this.selectedItemCount(this.list) < this.list.length) {
        this.loopListActive(true);
      } else {
        this.loopListActive(false);
      }

      this.allSelectActive = true;
    },
    /**
     * 单选按钮事件；
     * 如果有选中，将全选按钮的状态改变；
     * 如果单选按钮被全部选中，“全选”按钮也变成选中状态；
     * 如果没有选中按钮，将当前点击的按钮设置为选中状态；
     */
    selectItem(index) {
      this.list[index].active = !this.list[index].active;
      if (this.selectedItemCount(this.list) != 0) {
        this.allSelectActive = false;
      } else {
        this.list[index].active = true;
      }
      if (this.selectedItemCount(this.list) == this.list.length) {
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
#selectAll_02 {
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

