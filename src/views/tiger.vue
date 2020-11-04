<template>
  <div class="page">
    <div class="tiger">
      <section class="box">
        <div class="window-contain">
          <div class="window" v-for="(e, i) in 3" :key="i">
            <ul :ref="`window${i + 1}`">
              <li v-for="(item, index) in list" :key="index">
                <img :src="item" />
              </li>
            </ul>
          </div>
        </div>

        <div class="line"></div>
      </section>
      <section class="handle">
        <div class="fixed"></div>
        <div class="short"></div>
        <div ref="long" class="long"></div>
        <div ref="circle" class="circle" @click="begin()"></div>
      </section>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      list: [
        require("../assets/egg.svg"),
        require("../assets/pay.svg"),
        require("../assets/veg.svg"),
        require("../assets/tomato.svg"),
        require("../assets/fish.svg"),
        require("../assets/corn.svg"),
      ],
    };
  },

  computed: {
    resetLength() {
      return 6 * 5 * 120 - 120;
    },
  },

  methods: {
    // 初始化滚动列表
    initList() {
      let arr = [];
      for (let i = 0; i < 5; i++) {
        arr = arr.concat(...this.list);
      }
      this.list = [...arr];
    },

    resetWindow() {
      for (let i = 1; i <= 3; i++) {
        this.$refs[
          `window${i}`
        ].style.transform = `translateY(-${this.resetLength}px)`;
      }
    },

    // 摇动转臂
    begin() {
      this.handleRoll();
      this.windowRoll();
    },

    // 转臂动画
    handleRoll() {
      this.$refs.long.style.height = "30px";
      this.$refs.long.style.top = "122px";
      this.$refs.circle.style.top = "90px";

      setTimeout(() => {
        this.$refs.long.style.height = "100px";
        this.$refs.long.style.top = "52px";
        this.$refs.circle.style.top = "20px";
      }, 1000);
    },

    // 窗口滚动动画
    windowRoll() {
      for (let i = 1; i <= 3; i++) {
        this.$refs[`window${i}`].style.transition = `${1 + i}s`;
        let translateLength = `translateY(${
          this.getRndInteger(1, 6) * 120 + 6 * 3 * 120 - this.resetLength
        }px)`;
        this.$refs[`window${i}`].style.transform = translateLength;
      }
    },

    // 抽取随机
    // suffer(arr) {
    //   for (let i = 1; i < arr.length; i++) {
    //     const random = Math.floor(Math.random() * (i + 1));
    //     [arr[i], arr[random]] = [arr[random], arr[i]];
    //     return arr;
    //   }
    // },

    getRndInteger(min, max) {
      return Math.floor(Math.random() * (max - min + 1)) + min;
    },
  },

  mounted() {
    this.resetWindow();
  },

  created() {
    this.initList();
  },
};
</script>

<style lang="less" scoped>
.page {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  width: 100vw;
  background: #e0eefc;
}

.tiger {
  position: relative;
  width: 350px;
  height: 350px;
  border: 1px solid #d9d9d9;
  .box {
    position: absolute;
    height: 280px;
    width: 280px;
    background: #6573e1;
    border-radius: 40px;
    z-index: 4;
  }
}

.window-contain {
  position: absolute;
  left: 50%;
  top: 20px;
  transform: translateX(-50%);
  // margin: 0 auto;
  display: flex;
  // justify-content: space-between;
  align-items: center;
  height: 140px;
  width: 250px;
  background: #7d8ff2;
  border-radius: 15px;

  .window {
    margin-left: 10px;
    height: 120px;
    width: 70px;
    border-radius: 15px;
    background: #e9edfd;
    overflow: hidden;

    ul {
      margin: 0;
      padding: 0;
    }

    li {
      height: 120px;
      list-style: none;
      padding: 0 13px;

      img {
        width: 44px;
        height: 120px;
      }
    }
  }
}

.line {
  position: absolute;
  left: 50%;
  top: 200px;
  transform: translateX(-50%);
  height: 10px;
  width: 150px;
  background: #4c54d8;
  border-radius: 8px;
}

.handle {
  position: absolute;
  width: 40px;
  height: 280px;
  z-index: 3;
  .fixed {
    position: absolute;
    left: 260px;
    top: 125px;
    width: 40px;
    height: 40px;
    border-radius: 50%;
    background: #4c64d8;
    z-index: 2;
  }

  .short {
    position: absolute;
    left: 290px;
    top: 137px;
    height: 15px;
    width: 30px;
    background: #7d8ff2;
    z-index: 1;
  }

  .long {
    position: absolute;
    left: 313px;
    top: 52px;
    height: 100px;
    width: 15px;
    background: #7d8ff2;
    transition: 1s;
  }

  .circle {
    position: absolute;
    left: 300px;
    top: 20px;
    width: 40px;
    height: 40px;
    border-radius: 50%;
    background: #e86b67;
    cursor: pointer;
    transition: 1s;
  }
}
</style>