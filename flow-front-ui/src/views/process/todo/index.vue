<template>
  <PageWrapper title="流程中心" class="!mt-4 process-list-container">

    <template #extra>
      <launch-button />
    </template>

    <template #footer>
      <process-header current="todo"/>
    </template>

    <div class="mt-2 desc-wrap process">
      <BasicTable @register="registerTodoTable" >
        <template #bodyCell="{ column, record }">
          <template v-if="column.key === 'formName'">
            <router-link :to="`/process/approve/${record.processDefinitionKey}?taskId=${record.taskId}&procInstId=${record.processInstanceId}&businessKey=${record.businessKey}`"> {{record.formName}} </router-link>
          </template>
        </template>
      </BasicTable>
    </div>
  </PageWrapper>
</template>
<script lang="ts">
  import { defineComponent } from 'vue';
  import { BasicTable, useTable } from '/@/components/Table';
  import { PageWrapper } from '/@/components/Page';
  import { Tabs, Tag, Popover } from 'ant-design-vue';

  import { todoTableSchema, searchFormSchema } from './data';
  import ProcessHeader from '/@/views/process/components/ProcessHeader.vue';
  import LaunchButton from '/@/views/process/components/LaunchButton.vue';
  import { getAppingTasksPagerModel, getApps } from "/@/api/process/process";

  export default defineComponent({
    components: {
      BasicTable,
      ProcessHeader,
      LaunchButton,
      PageWrapper,
      [Tabs.name]: Tabs,
      [Tabs.TabPane.name]: Tabs.TabPane,
    },
    setup() {

      const [registerTodoTable, { getForm }] = useTable({
        api: getAppingTasksPagerModel,
        title: '',
        columns: todoTableSchema,
        formConfig: {
          labelWidth: 120,
          schemas: searchFormSchema,
          showAdvancedButton: false,
          showResetButton: false,
          autoSubmitOnEnter: true,
        },
        useSearchForm: true,
        pagination: true,
        showIndexColumn: true,
        canResize: false,
      });

      getApps().then(res=>{
        const {updateSchema} = getForm();
        updateSchema({
          field: 'appSn',
          componentProps: { options: res },
        })
      })


      return {
        registerTodoTable,
      };
    },
  });
</script>
<style lang="less">
.process-list-container{
  .vben-basic-table-form-container{
    padding: 0!important;
  }
}
  .process{
    .vben-basic-table-form-container{
      .ant-form{
        margin-bottom:0;
      }
    }
  }
</style>
