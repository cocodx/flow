<template>
  <Tooltip >
    <template #title>查看流程图</template>
    <Button type="link" @click="showFlowDiagram">
      <template #icon>
        <ApartmentOutlined />
      </template>
    </Button>
  </Tooltip>
  <BpmnPreviewModal @register="registerBpmnPreviewModal" />
</template>
<script lang="ts">
  import { defineComponent, ref, unref } from 'vue';
  import { ApartmentOutlined } from '@ant-design/icons-vue';
  import { Button, Tooltip } from 'ant-design-vue';
  import { useRouter } from 'vue-router';

  import { useModal } from '/@/components/Modal';
  import BpmnPreviewModal from '/@/views/components/preview/bpmnPreview/index.vue';

  export default defineComponent({
    name: 'BaseActionButtons',
    components: {
      Button, Tooltip,
      ApartmentOutlined,
      BpmnPreviewModal,
    },
    setup(_, {emit}) {
      const { currentRoute } = useRouter();

      const { params: {modelKey}, query : {taskId, procInstId} } = unref(currentRoute);

      const [registerBpmnPreviewModal, { openModal: openBpmnPreviewModal, setModalProps: setBpmnPreviewProps }] = useModal();

      function showFlowDiagram(){
        openBpmnPreviewModal(true, {
          modelKey: modelKey,
          procInstId: procInstId||'',
          isUpdate: true,
        });
        setBpmnPreviewProps({
          bodyStyle: {padding: '0px', margin: '0px'},
          width: 900, height: 400,
          showOkBtn: false, showCancelBtn: true,
          cancelText: '关闭'
        });
      }

      function handleCloseFunc() {
        return Promise.resolve(true);
      }

      return {
        registerBpmnPreviewModal,
        handleCloseFunc,
        showFlowDiagram,
      };
    },
  });
</script>
