<template>
  <div>
    <ag-grid-vue
      class="table ag-theme-balham ant-card-body"
      :editable="true"
      :enableRowGroup="true"
      :enablePivot="true"
      :enableValue="true"
      :defaultColDef="defaultColDef"
      :floatingFilter="true"
      :columnDefs="columnDefs"
      :rowData="rowData"
      rowSelection="multiple"
      @gridReady="onGridReady"
      @cellEditingStopped="onCellEditingStopped"
      @rowClicked="onRowClicked"
     />
    <div @click="reset">重设</div>
  </div>
</template>
<script type="text/javascript">
/* eslint-disable */
/***
 *  defaultColDef: { 
        enableRowGroup: true,
        enablePivot: true,
        enableValue: true,
        sortable: true, //开启排序 
    }, 
      //************************行内点击触发事件****************
    onRowClicked: function(event) {
        //event.data 选中的行内数据，event.event 为鼠标事件，等其他。可以log自己看一下
          var itxst = JSON.stringify(event.data);
          alert(itxst);
    },
        //************************单击单元格触发的事件****************
    onCellClicked: function(event) {
        //event.data 选中的行内数据，event.event 为鼠标事件，等其他。可以log自己看一下
          var itxst = JSON.stringify(event.data);
          alert(itxst);
    },
    //************************双击单元格触发的事件****************
    onCellDoubleClicked: function(event) {
        //event.data 选中的行内数据，event.event 为鼠标事件，等其他。可以log自己看一下
          var itxst = JSON.stringify(event.data);
          alert(itxst);
    },
 * editType="fullRow"是否开启行编辑
 *  //行编辑完成事件
     onRowEditingStopped: function (event) {
        var itxst = JSON.stringify(event.data);
        //alert(itxst);
    },
    //单元格编辑完成事件 
    onCellEditingStopped: function (event) {
        var itxst = JSON.stringify(event.data);
        alert(itxst);
*   }, 
  rowSelection: 'multiple', //设置多行选中
  rowMultiSelectWithClick:true, //单击就选中，如果为false需要按ctrl再点击才能选中多行
  rowHeight :30, //设置行高为30px,默认情况下是25px
  headerHeight:60, //设置表头的高度  
     //**************设置置顶行**************
    pinnedTopRowData:pinnedTopRowData,
    //**************设置置顶行样式**********
    getRowStyle: function (params) {
        if (params.node.rowPinned) {
            return {'font-weight': 'bold','color':'red'};
        }
    },
     //**************设置固定在底部的行**************
    pinnedBottomRowData:pinnedBottomRowData,
    //**************设置固定行样式**********
    getRowStyle: function (params) {
        if (params.node.rowPinned) {
            return {'font-weight': 'bold','color':'red'};
        }
    },
    onGridReady: function () {
         //表格创建完成后执行的事件
        gridOptions.api.sizeColumnsToFit();//调整表格大小自适应
    },
 *  */

import { AgGridVue } from "ag-grid-vue";
export default {
  name: "ag_grid_page",
  components: { AgGridVue },
  data() {
    return {
      columnDefs: [
        // {
        //   headerName: "", //选择列头部显示的文字，可以为空
        //   checkboxSelection: true, //设置为ture显示为复选框
        //   headerCheckboxSelection: true, //表头是否也显示复选框，全选反选用
        //   pinned: "left", //固定再左边
        //   width: 50 //列的宽度
        // },
        {
          headerName: "姓名",
          field: "name",
          pinned: "left",
          filter: "agTextColumnFilter"
        },
        {
          headerName: "性别",
          field: "sex",
          cellEditor: "agSelectCellEditor", //编辑时 显示下拉列表**************
          cellEditorParams: { values: ["男", "女"] }
        },
        { headerName: "年龄", field: "age" },
        { headerName: "籍贯", field: "jg" },
        { headerName: "省份", field: "sf" },
        {
          headerName: "地址",
          field: "dz",
          cellEditor: "agLargeTextCellEditor", //编辑时 显示长文本框**************
          cellEditorParams: {
            maxLength: "300",
            cols: "50",
            rows: "6"
          }
        }
      ],
      rowData: [
        {
          name: "张三",
          sex: "男",
          age: "100",
          jg: "中国",
          sf: "浙江",
          dz: "杭州市古墩路1号"
        },
        {
          name: "李四",
          sex: "女",
          age: "5",
          jg: "中国",
          sf: "浙江",
          dz: "杭州市古墩路12号"
        },
        {
          name: "王五",
          sex: "女",
          age: "20",
          jg: "中国",
          sf: "浙江",
          dz: "杭州市古墩路31号"
        },
        {
          name: "王五",
          sex: "女",
          age: "26",
          jg: "中国",
          sf: "浙江",
          dz: "杭州市古墩路111号"
        },
        {
          name: "王五",
          sex: "男",
          age: "35",
          jg: "中国",
          sf: "浙江",
          dz: "杭州市古墩路12号"
        },
        {
          name: "王五",
          sex: "男",
          age: "35",
          jg: "中国",
          sf: "浙江",
          dz: "杭州市古墩路12号"
        },
        {
          name: "王五",
          sex: "男",
          age: "35",
          jg: "中国",
          sf: "浙江",
          dz: "杭州市古墩路12号"
        },
        {
          name: "王五",
          sex: "男",
          age: "35",
          jg: "中国",
          sf: "浙江",
          dz: "杭州市古墩路12号"
        },
        {
          name: "王五",
          sex: "男",
          age: "35",
          jg: "中国",
          sf: "浙江",
          dz: "杭州市古墩路12号"
        },
        {
          name: "王五",
          sex: "男",
          age: "35",
          jg: "中国",
          sf: "浙江",
          dz: "杭州市古墩路12号"
        },
        {
          name: "王五",
          sex: "男",
          age: "35",
          jg: "中国",
          sf: "浙江",
          dz: "杭州市古墩路12号"
        },
        {
          name: "王五",
          sex: "男",
          age: "35",
          jg: "中国",
          sf: "浙江",
          dz: "杭州市古墩路12号"
        },
        {
          name: "王五",
          sex: "男",
          age: "35",
          jg: "中国",
          sf: "浙江",
          dz: "杭州市古墩路12号"
        },
        {
          name: "王五",
          sex: "男",
          age: "35",
          jg: "中国",
          sf: "浙江",
          dz: "杭州市古墩路12号"
        },
        {
          name: "王五",
          sex: "男",
          age: "35",
          jg: "中国",
          sf: "浙江",
          dz: "杭州市古墩路12号"
        }
      ],
      defaultColDef: {
        editable: true //单元表格是否可编辑
      }
    };
  },
  mounted() {},
  methods: {
    onCellEditingStopped(e){console.log(e)},
    onRowClicked(event){ 
    },
    // 表格创建完成后执行
    onGridReady(params) { 
      // 获取gridApi
      this.gridApi = params.api;
      this.columnApi = params.columnApi;
      // 调整表格列宽大小自适应
      // this.gridApi.sizeColumnsToFit();
    },

    reset() {
      this.rowData = [
        {
          name: "新的数据",
          sex: "男",
          age: "100",
          jg: "中国",
          sf: "长沙",
          dz: "长沙市岳麓区"
        }
      ];
    }
  }
};
</script>
<style scoped>
.ag-row-hover {
  background-color: green !important;
} 
.ag-column-hover {
  background-color: red;
} 
.table { 
  height: 500px;
}
</style>
