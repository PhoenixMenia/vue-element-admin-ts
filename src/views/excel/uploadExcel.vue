<template>
  <div class="app-container">
    <upload-excel-component :on-success="handleSuccess" :before-upload="beforeUpload"/>
    <el-table :data="tableData" border highlight-current-row style="width: 100%;margin-top:20px;">
      <el-table-column v-for="item of tableHeader" :prop="item" :label="item" :key="item"/>
    </el-table>
  </div>
</template>

<script lang="ts">
  import { Component, Vue } from 'vue-property-decorator';
  import { UploadExcelComponent } from '@/components';

  @Component({
    components: {
      UploadExcelComponent
    }
  })
  export default class UploadExcelView extends Vue {
    tableData: any[] = [];
    tableHeader: any[] = [];

    beforeUpload(file) {
      const isLt1M = file.size / 1024 / 1024 < 1;

      if (isLt1M) {
        return true;
      }

      this.$message({
        message: 'Please do not upload files larger than 1m in size.',
        type: 'warning'
      });
      return false;
    }

    handleSuccess({results, header}) {
      this.tableData = results;
      this.tableHeader = header;
    }
  }
</script>
