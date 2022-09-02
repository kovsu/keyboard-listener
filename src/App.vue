<script setup lang="ts">
import { reactive, ref } from "vue";

const records = reactive<string[]>([]);
const msg = ref("");
let timer: number;

window.addEventListener("keydown", (e) => {
  if (timer) {
    clearTimeout(timer);
    if (e.keyCode === 32) {
      msg.value += "Space ";
    } else {
      msg.value += e.key + " ";
    }
    // console.log(e.keyCode);
  }

  timer = setTimeout(() => {
    console.log("输入结束");
    records.length = 0;
    msg.value = "";
  }, 2000);

  if (e.keyCode === 13) {
    clearTimeout(timer);
    if (msg.value === "") {
      records.push("13");
    } else {
      records.push(msg.value.split(" ").slice(0, -1).join("+"));
    }
    msg.value = "";

    timer = setTimeout(() => {
      records.length = 0;
    }, 2000);
  }
});
</script>

<template>
  <TransitionGroup name="list" tag="div" class="record">
    <div v-for="item in records" :key="item">
      <p>
        <span class="keycap" v-for="key in item.split('+')">
          {{ key }}
        </span>
      </p>
    </div>
  </TransitionGroup>

  <div class="command" v-show="msg !== ''">
    <span class="keycap" v-for="item in msg.split(' ').slice(0, -1)">
      {{ item }}
    </span>
  </div>
</template>

<style scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.record {
  position: fixed;
  width: 300px;
  height: auto;
  display: flex;
  flex-direction: column;
}

.record > div {
  width: 100%;
}

.record > div:not(:last-child) {
  margin-bottom: 10px;
}

.record > div p {
  max-width: 300px;
  width: max-content;

  background-color: rgba(0, 0, 0, 0.8);
  border-radius: 10px;
  padding: 10px;
  color: #fff;

  display: flex;
  flex-wrap: wrap;
  gap: 5px;
  justify-content: center;
}

.command {
  position: fixed;
  right: 20px;
  top: 20px;

  max-width: 300px;
  width: auto;
  height: auto;
  padding: 10px;
  border-radius: 10px;
  background-color: rgba(0, 0, 0, 0.8);

  color: #fff;
  font-size: 24px;

  display: flex;
  flex-wrap: wrap;
  gap: 5px;
  justify-content: center;
}

.list-move, /* 对移动中的元素应用的过渡 */
.list-enter-active,
.list-leave-active {
  transition: all 0.5s ease;
}

.list-enter-from {
  opacity: 0;
  transform: translateY(30px);
}

.list-leave-to {
  opacity: 0;
  transform: translateY(-100px);
}

/* 确保将离开的元素从布局流中删除
  以便能够正确地计算移动的动画。 */
/* .list-leave-active {
  position: absolute;
} */

.keycap {
  color: #585858;
  background: rgb(255, 255, 255);
  border-color: #e4e4e4;
  border-radius: 4px;
  box-shadow: 0 1px 0 #dadadd;
  line-height: 1;
  padding: 6px 10px;

  font-size: 20px;
}
</style>
