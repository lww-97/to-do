<template>
  <div>
    <h1 class="content">My Todos</h1>
    <div class="content">
      <el-input v-model="input" placeholder="请输入内容"></el-input>
      <el-button type="primary" class="button" @click="save()">Save</el-button>
    </div>
    <ul>
      <li v-for="(item, index) in list" :key="item.id">
        <div class="form">
          <div class="left">
            <el-checkbox
              :value="item.checked"
              @change="changeBox(item)"
            ></el-checkbox>
            <span
              v-if="item.isText"
              @click="clickInput(item, index)"
              class="textWidth"
              >{{ item.text }}</span
            >
            <el-input
              :ref="index"
              v-model="item.text"
              v-else
              @blur="clickBlur(item)"
              class="input"
            ></el-input>
          </div>
          <div class="right">
            <el-button type="primary" v-if="!item.isText" @click="update(item)"
              >Save</el-button
            >
            <el-button type="danger" @click="remove(item.id)">remove</el-button>
          </div>
        </div>
      </li>
    </ul>
  </div>
</template>
<script>
export default {
  data() {
    return {
      input: "",
      list: [],
    };
  },
  mounted() {
    let arr = localStorage.getItem("todoList");
    this.list = JSON.parse(arr);
  },
  watch: {
    list: {
      handler() {
        localStorage.setItem("todoList", JSON.stringify(this.list));
      },
      deep: true,
    },
  },
  methods: {
    save() {
      if (!this.input.trim()) {
        return;
      }
      this.list.push({
        id: Math.random(),
        text: this.input,
        checked: false,
        isText: true,
      });
      this.input = "";
    },
    changeBox(info) {
      info.checked = !info.checked;
    },
    clickInput(info) {
      info.isText = false;
    },
    clickBlur(info) {
      info.isText = true;
    },
    update(info) {
      info.isText = true;
    },
    remove(id) {
      this.list = this.list.filter((item) => item.id !== id);
    },
  },
};
</script>
<style lang="less">
.content {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 500px;
  margin-left: 30%;
  margin-bottom: 30px;
  .button {
    margin-left: 5px;
  }
  .el-checkbox__inner {
    margin-right: 5px;
  }
}
ul {
  width: 500px;
  margin-left: 30%;
  padding: 0;
}
li {
  list-style: none;
  padding: 10px 0px;
  border-top: 1.5px solid #ddd;
}

.form {
  margin-top: 10px;
  display: flex;
  justify-content: space-between;
  .left {
    display: flex;
    justify-content: left;
    align-items: center;
    .textWidth {
      margin-left: 10px;
    }
    .input {
      margin-left: 10px;
      width: 295px;
    }
  }

  .right {
    display: flex;
    justify-content: right;
    align-items: center;
  }
}
</style>
