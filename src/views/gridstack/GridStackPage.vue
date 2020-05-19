<template>
  <div style="background:#e8e8e8;padding:16px;">
    <div class style="text-align:right;padding-right:12px; padding-bottom:10px;">
      <el-button type="primary" @click="saveItemDataSet">保存布局</el-button>
    </div>
    <div class="grid-stack" data-gs-animate="yes" style="height:460px">
      <div
        v-for="(item,index) in data"
        :key="index"
        class="grid-stack-item"
        :data-gs-x="item.dataSetX"
        :data-gs-y="item.dataSetY"
        :data-gs-width="item.dataSetW"
        :data-gs-min-height="3"
        :data-gs-min-width="3"
        :data-gs-height="item.dataSetH"
      >
        <div class="grid-stack-item-content">
          <common-card>
            <template slot="slotHeader">
              <el-row :gutter="20" style="line-height:50px;">
                <el-col :span="11" :offset="1" class="slotHeaderLeft">{{ item.title }}</el-col>
                <el-col :span="11" class="slotHeaderRight">
                  <svg-icon
                    :icon-class="item.display?'fullscreen':'exit-fullscreen'"
                    @click="maxSize(item,index)"
                  />
                  <i class="el-icon-more" />
                </el-col>
                <el-col :span="1" />
              </el-row>
            </template>
            <template slot="slotBody">
              <div class="card-panel">
                <div class="card-panel-icon-wrapper icon-money">
                  <svg-icon icon-class="money" class-name="card-panel-icon" />
                </div>
                <div class="card-panel-description">
                  <div class="card-panel-text">Purchases</div>
                  <count-to :start-val="0" :end-val="9280" :duration="3200" class="card-panel-num" />
                </div>
              </div>
            </template>
            <template slot="slotFooter">
              <el-row style="line-height:50px;">
                <el-col :span="8" style="border-right:1px solid #eee;">分享</el-col>
                <el-col :span="8" style="border-right:1px solid #eee;">编辑</el-col>
                <el-col :span="8" >删除</el-col>
              </el-row>
            </template>
          </common-card>
        </div>
      </div>
    </div>
  </div>
</template>
<script type="text/javascript">
/* eslint-disable */
import demoData from "@/store/modules/demo-data";
import CommonCard from "@/components/Card/CommonCard";
import CountTo from "vue-count-to";
export default {
  name: "grid_stack_page",
  components: {
    CommonCard,
    CountTo
  },
  data() {
    return {
      data: [],
      modalVisible: false,
      grid: {}
    };
  },
  mounted() {
    this.getList();
  },
  activated() {
    this.getList();
  },
  methods: {
    async getList() {
      // let res = await this.$http.get('/list/article', { params: { count: 8 } })
      // console.log(JSON.stringify(res))
      this.data = demoData.articleList.result;
      this.data.map((item, index) => {
        item.display = true;
        item.dataSetX = (index % 3) * 4;
        item.dataSetY = Math.floor(index / 3) * 3;
        item.dataSetW = 4;
        item.dataSetH = 3;
      });
      this.loading = false;
      this.$nextTick(() => {
        this.grid = GridStack.init();
        this.grid.addWidget("", { width: 2 });
        this.grid.on("change", function(event, items) {
          console.log(event);
          console.log(items);
        });
      });
    },
    saveItemDataSet() {
      console.log(this.grid);
    },
    maxSize(item, e) {
      if (item.display) {
        this.grid.update(this.grid.el.children[e], 0, item.dataSetY, 12, 3);
      } else {
        this.grid.update(
          this.grid.el.children[e],
          item.dataSetX,
          item.dataSetY,
          4,
          3
        );
      }
      item.display = !item.display;
      this.$forceUpdate();
    }
  }
};
</script>
<style lang="scss" scoped>
@import url(grid_stack_page.css);
.slotHeaderLeft {
  text-align: left;
  padding-left: 30px;
}
.slotHeaderRight {
  text-align: right;
  padding-right: 30px;
}
.card-panel {
  height: 100%;
  cursor: pointer;
  font-size: 12px;
  width: 100%;
  color: #666;
  background: #fff;
  box-shadow: 4px 4px 40px rgba(0, 0, 0, 0.05);
  border-color: rgba(0, 0, 0, 0.05);

  &:hover {
    .card-panel-icon-wrapper {
      color: #fff;
    }

    .icon-people {
      background: #40c9c6;
    }

    .icon-message {
      background: #36a3f7;
    }

    .icon-money {
      background: #f4516c;
    }

    .icon-shopping {
      background: #34bfa3;
    }
  }

  .icon-people {
    color: #40c9c6;
  }

  .icon-message {
    color: #36a3f7;
  }

  .icon-money {
    color: #f4516c;
  }

  .icon-shopping {
    color: #34bfa3;
  }

  .card-panel-icon-wrapper {
    float: left;
    margin: 14px 0 0 14px;
    padding: 16px;
    transition: all 0.38s ease-out;
    border-radius: 6px;
  }

  .card-panel-icon {
    float: left;
    font-size: 48px;
  }

  .card-panel-description {
    float: right;
    font-weight: bold;
    margin: 26px;
    margin-left: 0px;

    .card-panel-text {
      line-height: 18px;
      color: rgba(0, 0, 0, 0.45);
      font-size: 16px;
      margin-bottom: 12px;
    }

    .card-panel-num {
      font-size: 20px;
    }
  }
}
</style>
