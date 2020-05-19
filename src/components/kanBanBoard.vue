<template>
  <div class="drag-container"> 
      <draggable
        v-model="list"
        class="drag-list"
        draggable=".drag-column"
        tag="ul"
        v-bind="dragOptions"
        :move="checkStageMove"
        @update="stageDragUpdate"
      >
        <li
          v-for="stage in list"
          :key="stage.stage"
          :class="stage.stage=='on-hold'?'drag-column1':'drag-column'"
          :data-stage="stage.stage"
        >
          <span class="drag-column-header">
            <slot :name="stage.stage" />
          </span>
          <draggable
            class="list-group"
            tag="ul"
            :list="stage.blocks"
            draggable=".drag-item"
            v-bind="itemDragOptions"
            :move="checkBlockMove"
            @end="itemStageDragEnd"
          >
            <transition-group type="transition" name="flip-list" :data-stage="stage.stage">
              <li
                v-for="(block) in stage.blocks"
                :key="block.id"
                :class="block.id=='1'?'drag-item1':'drag-item'"
                :data-block-id="block.id"
                :data-stage="stage.stage"
              >
                <slot :name="block.id">
                  <strong>{{ block.status }}</strong>
                  <div>{{ block.id }}</div>
                </slot>
              </li>
              <li
                v-if="stage.count==0"
                :data-block-id="stage.stage"
                :data-stage="stage.stage"
                :key="stage.stage"
                class="drag-item"
                style="background-color:rgb(247, 247, 247);box-shadow:none;padding:1px;"
              ></li>
              <div :key="`footer-${stage.stage}`" class="drag-column-footer">
                <slot :name="`footer-${stage.stage}`" />
              </div>
            </transition-group>
          </draggable>
        </li>
      </draggable>
  </div>
</template>

<script>
/* eslint-disable */
import draggable from "vuedraggable";
export default {
  name: "KanbanBoard",

  props: {
    StagesList: {
      type: Array,
      required: true
    },
    config: {
      type: Object,
      default: () => ({})
    },
    stateMachineConfig: {
      type: Object,
      default: null
    }
  },
  components: {
    draggable
  },
  data() {
    return {
      list: [],
      machine: null,
      stageNotic: null,
      blockNotic: null
    };
  },
  created() {
    this.list = this.StagesList;
  },
  activated() {
    this.list = this.StagesList;
  },
  computed: {
    dragOptions() {
      return {
        animation: 1000,
        group: "description",
        disabled: false,
        ghostClass: "ghost"
      };
    },
    itemDragOptions() {
      return {
        animation: 1000,
        group: "item",
        disabled: false,
        ghostClass: "ghost"
      };
    },
    localBlocks() {
      return this.blocks;
    }
  },

  methods: {
    syncStageDrag() {
      this.list = this.StagesList;
    },
    checkStageMove(a, b, c) {
      if (
        a.dragged.dataset.stage == "in-progress" &&
        a.related.dataset.stage == "approved"
      ) {
        if (
          this.stageNotic == null ||
          (this.stageNotic && this.stageNotic.closed == true)
        ) {
          this.stageNotic = this.$notify.info({
            title: "警告提示",
            message: "处理中任务不能在完成状态之后",
            duration: 2000,
            offset: 100
          });
        }
        return false;
      } else {
        return true;
      }
    },
    checkBlockMove(a, b, c) {
      if (
        (a.dragged.dataset.blockId == "2" &&
          a.related.dataset.blockId == "3") ||
        (a.dragged.dataset.blockId == "3" && a.related.dataset.blockId == "2")
      ) {
        if (
          this.blockNotic == null ||
          (this.blockNotic && this.blockNotic.closed == true)
        ) {
          this.blockNotic = this.$notify.info({
            title: "警告提示",
            message: "2号不能与3号互换位置",
            duration: 2000,
            offset: 100
          });
        }
        return false;
      } else {
        return true;
      }
    },
    itemStageDragEnd(a) {
      if (!!a.item.dataset.blockId) {
        this.$emit("UpdateBlocks", {
          targetIndex: a.newIndex,
          sourceIndex: a.oldIndex,
          blockId: a.item.dataset.blockId,
          sourceStage: a.item.dataset.stage,
          targetStage: a.to.dataset.stage
        });
      }
    },
    stageDragUpdate(a) {
      this.$emit(
        "UpdataStages",
        this.StagesList[a.newIndex].stage,
        this.StagesList[a.oldIndex].stage
      );
    },
    getBlocks(status) {
      let arry = [];
      this.blocks.map(item => {
        if (item.status === status) {
          arry.push(item);
        }
      });
      return arry;
    }
  }
};
</script>
<style scoped>
.button {
  margin-top: 35px;
}
.flip-list-move {
  transition: transform 0.5s;
}
.no-move {
  transition: transform 0s;
}
.ghost {
  opacity: 0.5;
  background: #c8ebfb;
}
.list-group {
  min-height: 20px;
}
.list-group-item {
  cursor: move;
}
.list-group-item i {
  cursor: pointer;
}
</style>