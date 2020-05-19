<template>
  <div style="" class="kanban">
    <kan-ban-board
      ref="draggableRef"
      :stages="Stages"
      :stages-list="StagesList"
      @UpdateBlocks="updateBlocks"
      @UpdataStages="updataStages"
    >
      <div v-for="(stageItem,index) in StagesList" :slot="stageItem.stage" :key="index">
        <h2>
          <span v-if="stageItem.stage=='on-hold'" class="task-title">{{ stageItem.title }}(不可拖拽)</span>
          <span v-else class="task-title">{{ stageItem.title }}</span>
          <span class="task-count">{{ stageItem.count }}</span>
          <a style="color:#8c8c8c" @click="action(stageItem)">...</a>
        </h2>
      </div>
      <div v-for="item in blocks" :slot="item.id" :key="item.id" class="task">
        <div class="task-priority" data-title="非常紧急" />
        <div class="item-content">
          <el-checkbox v-if="item.id==1" disabled :checked="true" />
          <el-checkbox v-else />
          <div v-if="item.id==1" style="color:#262626;font-size:14px;">{{ item.title }}(不可拖拽)</div>
          <div v-else style="color:#262626;font-size:14px;">{{ item.title }}</div>
        </div>
        <div style="padding-left:20px">
          <el-button style="color:#262626">操作一</el-button>
          <el-button style="color:#262626">操作二</el-button>
        </div>
      </div>
      <div
        v-for="stage in StagesList"
        :key="stage.stage"
        :slot="`footer-${stage.stage}`"
        class="drag-add"
        style="margin:0px"
        @click="addBlock(stage.stage)"
      >
        <span>+</span>
      </div>
    </kan-ban-board>
    <div class="addStage" id="addStage" @click="addStage">
      <div class="addTag">
        <h1>+</h1>
      </div>
    </div>
  </div>
</template>
<script>
/* eslint-disable */
import kanBanBoard from "@/components/kanBanBoard.vue";
import { debounce } from "lodash";

export default {
  name: "kanBan",
  components: {
    kanBanBoard
  },
  data() {
    return {
      Stages: ["on-hold", "in-progress", "needs-review", "approved"],
      StagesList: [
        {
          stage: "on-hold",
          count: 0,
          title: "待处理",
          blocks: []
        },
        {
          stage: "in-progress",
          count: 0,
          title: "处理中",
          blocks: []
        },
        { stage: "needs-review", count: 0, title: "协助中", blocks: [] },
        { stage: "approved", count: 0, title: "已完成", blocks: [] }
      ],
      blocks: []
    };
  },
  mounted() {
    for (let i = 0; i <= 10; i += 1) {
      let rom = Math.floor(Math.random() * 4);
      let obj = {
        id: i,
        status: this.Stages[rom],
        title: i + ". 将撰写中的产品提案移动到这个状态"
      };
      if (obj.id == 2) {
        obj.title = "2.我不能和3号互换位置！";
      }
      if (obj.id == 3) {
        obj.title = "3.我不能和2号互换位置！";
      }
      this.blocks.push(obj);
      if (this.StagesList[rom].count == 0) {
        this.StagesList[rom].blocks = [];
      }
      this.StagesList[rom].blocks.push(obj);
      this.StagesList[rom].count++;
    }
  },
  methods: {
    addStage() {
      let stateName = "approved" + this.StagesList.length;
      this.StagesList.push({
        stage: stateName,
        count: 0,
        title: "测试"
      });
      this.$refs.draggableRef.syncStageDrag();
    },
    selectStyle(item, event) {
      console.log(item);
      console.log(event);
    },
    outStyle(item, event) {
      console.log(item);
      console.log(event);
    },

    action(item) {
      this.$Message.info("功能暂未开放！");
    },
    updataStages: debounce(function(targetStage, sourceStage) {
      let arry = [];
      this.StagesList.map(item => {
        if (item.stage == targetStage) {
          let obj = this.StagesList.filter(
            item => item.stage === sourceStage
          )[0];
          arry.push(obj);
        } else if (item.stage == sourceStage) {
          let obj = this.StagesList.filter(
            item => item.stage === targetStage
          )[0];
          arry.push(obj);
        } else {
          arry.push(item);
        }
      });
      this.StagesList = arry;
    }, 1000),
    updateBlocks: debounce(function(item) {
      // {
      //    targetIndex: 目标序列号
      //   sourceIndex: 原序列号,
      //   blockId: blockId,
      //   sourceStage: 原stage,
      //   targetStage: 目标stage
      // }
      let that = this;
      if (item.sourceStage === item.targetStage) {
        that.StagesList.map(stage => {
          if (stage.stage === item.targetStage) {
            let targetBlock = stage.blocks[item.targetIndex];
            let sourceBlock = stage.blocks[item.sourceIndex];
            stage.blocks.map((block, index) => {
              if (index === item.targetIndex) {
                block = sourceBlock;
              }
              if (index === item.sourceIndex) {
                block = targetBlock;
              }
            });
          }
        });
      } else {
        let sourceBlock = {};
        that.blocks.map(block => {
          if (block.id == item.blockId) {
            sourceBlock = block;
          }
        });

        that.StagesList.map(stage => {
          if (stage.stage === item.sourceStage) {
            let ary1 = [];
            stage.blocks.map((block, index) => {
              if (block.id !== item.blockId) {
                ary1.push(block);
              }
            });
            stage.blocks = ary1;
            stage.count--;
          } else if (stage.stage === item.targetStage) {
            let ary2 = [];
            if (stage.count === 0) {
              stage.blocks = [];
              stage.blocks.push(sourceBlock);
              stage.count++;
            } else {
              stage.blocks.map((block, index) => {
                if (index === item.targetIndex) {
                  ary2.push(sourceBlock);
                } else {
                  ary2.push(block);
                }
              });
              //console.log(ary2);
              stage.blocks = ary2;
              stage.count++;
            }
            // console.log(stage);
          }
        });
        that.$refs.draggableRef.syncStageDrag();
      }
    }, 1000),
    addBlock: debounce(function(stage) {
      let obj = {
        id: this.blocks.length,
        status: stage,
        title: this.blocks.length + ". 将撰写中的产品提案移动到这个状态"
      };
      this.StagesList.map(item => {
        if (item.stage === stage) {
          if (item.count === 0) {
            item.blocks = [];
          }
          item.count++;
          item.blocks.push(obj);
        }
      });
      this.blocks.push(obj);
      this.$refs.draggableRef.syncStageDrag();
    }, 1000)
  }
};
</script>

<style    scoped>
@import "./kanban.css";
</style>