<template>
  <div class="editor-box rel">
    <div class="line-box abs">
      <canvas ref="canvas" />
    </div>
    <div class="dom-box rel">
      <div
        v-for="(item, key) in list"
        :key="key"
        v-popover:popover
        class="dom-item abs"
        :style="{left: item.pos[0] + 'px', top: item.pos[1] + 'px'}"
      >
        <div class="dom-item-cont">
          <p slot="reference" @click="GetNodeInfo(item)">{{ item.title }}</p>

          <el-popover
            v-model="item.visible"
            placement="bottom"
            title="节点详情"
            width="300"
            trigger="manual"
            content
          >
            <el-card :body-style="{ padding: '0px' }">
              <img src="https://shadow.elemecdn.com/app/element/hamburger.9cf7b091-55e9-11e9-a976-7f4d0b07eef6.png" class="image" />
              <div style="padding: 14px;">
                <span>{{ currentInfo.title }}</span>
                <div class="bottom clearfix">
                  <time class="time">{{ currentDate }}</time>
                  <el-button type="text" class="button">其他</el-button>
                </div>
              </div>
            </el-card>
            <el-button slot="reference" type="text" @click="getDetailInfo(item)">详情</el-button>
          </el-popover>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
/* eslint-disable */
const HEIGHT = 1000;
let totalHeight = 150;
let pen = null;
const mousePos = {
  mx: 0,
  my: 0,
  ex: 0,
  ey: 0,
  ele: null
};
let list = [];
let tid = 10;
export default {
  components: {},
  data() {
    return {
      showMask: false,
      move: false,
      WIDTH: 1000,
      currentDate: "2020-05-15",
      visible: false,
      list: [],
      linePos: [],
      currentInfo: {}
    };
  },
  mounted() {
    this.init();
  },
  created() {
    list = [
      {
        id: 1,
        pid: 0,
        title: "1",
        level: 1,
        name: "1"
      },
      {
        id: 12,
        pid: 1,
        title: "12",
        level: 2,
        name: "12"
      },
      {
        id: 2,
        pid: 11,
        level: 3,
        title: "2",
        name: "2"
      },
      {
        id: 11,
        pid: 1,
        level: 2,
        title: "11",
        name: "11"
      },
      {
        id: 13,
        pid: 1,
        level: 2,
        title: "13",
        name: "13"
      },
      {
        id: 14,
        pid: 1,
        level: 2,
        title: "14",
        name: "14"
      },
      {
        id: 6,
        pid: 5,
        level: 3,
        title: "6",
        name: "6"
      },
      {
        id: 17,
        pid: 6,
        level: 4,
        title: "17",
        name: "17"
      },
      {
        id: 18,
        pid: 6,
        level: 4,
        title: "18",
        name: "18"
      },
      {
        id: 5,
        pid: 1,
        level: 2,
        title: "5",
        name: "5"
      },
      {
        id: 3,
        pid: 1,
        level: 2,
        title: "3",
        name: "3"
      },
      {
        id: 10,
        pid: 12,
        level: 3,
        title: "10",
        name: "10"
      },

      {
        id: 4,
        pid: 12,
        level: 3,
        title: "4",
        name: "4"
      },
      {
        id: 7,
        pid: 4,
        level: 4,
        title: "7",
        name: "7"
      },
      {
        id: 15,
        pid: 4,
        level: 4,
        title: "15",
        name: "15"
      },
      {
        id: 16,
        pid: 4,
        level: 4,
        title: "16",
        name: "16"
      },
      {
        id: 19,
        pid: 10,
        level: 4,
        title: "19",
        name: "19"
      },
      {
        id: 9,
        pid: 4,
        level: 4,
        title: "9",
        name: "9"
      },
      {
        id: 8,
        pid: 7,
        level: 5,
        title: "8",
        name: "8"
      }
    ];
    window.ve = this;
    this.initPos();
  },
  methods: {
    getDetailInfo(item) {
      item.visible = !item.visible;
      this.currentInfo = item;
      this.list.map(ele => {
        if (ele != item) {
          ele.visible = false;
        }
      });
    },
    getLevelIndex(node, list) {
      let ary = list.filter(item => item.level == node.level);
      let levelIndex = 0;
      ary.map((item, index) => {
        if (item == node) {
          levelIndex = index;
        }
      });
      return levelIndex;
    },
    initPos() {
      list = list.sort((a, b) => a.id - b.id);
      list = list.sort((a, b) => a.level - b.level);

      let length = [];
      let pid = 0;
      let levelIndex = 0;
      let that = this;
      list.forEach((ele, index) => {
        ele.visible = false;
        ele.pele = list.filter(item => item.id == ele.pid)[0];
        if (ele.pid == 0) {
          ele.sortOrder = "0";
          ele.sortOrder = "1";
        } else {
          ele.sortOrder = ele.level + ele.pele.sortOrder + index;
          console.log(ele.sortOrder);
        }
      });
      list = list.sort((a, b) => a.sortOrder - b.sortOrder);

      list.map(ele => {
        if (length[ele.level]) {
          length[ele.level]++;
        } else {
          length[ele.level] = 1;
        }
        ele.levelIndex = this.getLevelIndex(ele, list);
      });
      console.log(list);
      this.list = list.map(ele => {
        let level = ele.level;
        let len = length[level];
        let levelIndex = ele.levelIndex;
        let x = (this.WIDTH / len) * (levelIndex + 0.5);
        let y = totalHeight * level - 100;
        ele.pos = [x, y];
        return ele;
      });
    },
    init() {
      let canvas = this.$refs.canvas;
      canvas.width = this.WIDTH;
      canvas.height = HEIGHT;
      pen = canvas.getContext("2d");
      pen.strokeStyle = "#999";
      this.draw();
    },
    draw() {
      pen.clearRect(0, 0, this.WIDTH, HEIGHT);
      for (let i = 0; i < this.list.length; i++) {
        let end = this.list[i];
        let x1 = end.pos[0] + 60;
        let y1 = end.pos[1];
        let cx1 = x1;
        let cy1 = y1 - 60;
        let start = this.list[i].pele;
        if (start && end.pid === start.id) {
          let x2 = start.pos[0] + 60;
          let y2 = start.pos[1] + 60;
          let cx2 = x2;
          let cy2 = y2 + 60;
          pen.beginPath();
          pen.moveTo(x1, y1);
          //pen.quadraticCurveTo(cx1, cy1,   x2, y2);
          pen.bezierCurveTo(cx1, cy1, cx2, cy2, x2, y2);
          pen.stroke();
          pen.closePath();
        }
      }
    },
    minAndMax(min, value, max) {
      return value > max ? max : value < min ? min : value;
    },
    GetNodeInfo(item) {
      console.log(item);
    },
    mouseDown(event, key) {
      if (event.buttons !== 1) {
        return false;
      }
      this.showMask = true;
      mousePos.ex = event.pageX;
      mousePos.ey = event.pageY;
      mousePos.ele = this.list[key];
      mousePos.mx = mousePos.ele.pos[0];
      mousePos.my = mousePos.ele.pos[1];
    },
    reset() {
      this.initPos();
      this.draw();
    }
  }
};
</script>

<style lang="scss" scoped>
.time {
  font-size: 13px;
  color: #999;
}

.bottom {
  margin-top: 13px;
  line-height: 12px;
}

.button {
  padding: 0;
  float: right;
}

.image {
  width: 100%;
  display: block;
}

.clearfix:before,
.clearfix:after {
  display: table;
  content: "";
}

.clearfix:after {
  clear: both;
}
.rel {
  position: relative;
}
.abs {
  position: absolute;
}
.dom-box {
  width: 1000px;
  height: 1000px;
}
.dom-item {
  .item-abs-top,
  .item-abs-bottom {
    height: 20px;
    width: 20px;
    text-align: center;
    line-height: 20px;
    background: #fff;
    border: 1px solid #ddd;
    display: none;
    cursor: pointer;
  }
  .item-abs-top {
    right: 0;
    top: -20px;
  }
  .item-abs-bottom {
    left: 50%;
    margin-left: -10px;
    bottom: -20px;
  }
  &:hover {
    .item-abs-top,
    .item-abs-bottom {
      display: block;
    }
  }
  .dom-item-cont {
    padding: 10px;
    background: #f2f6fa;
    border: 1px solid #eee;
    width: 120px;
    height: 60px;
    text-align: center;
    overflow: hidden;
    line-height: 20px;
    cursor: pointer;
    p {
      margin: 0;
    }
  }
}
.editor-mask {
  position: fixed;
  left: 0;
  top: 0;
  right: 0;
  bottom: 0;
  z-index: 999;
  cursor: pointer;
  &.move {
    cursor: move;
  }
}
.ml-10 {
  margin-left: 10px;
}
</style>
