<template>
  <CommonTemplate>
    <template #left>
      <ResultCatalog />
    </template>
    <template #center>
      <ResultPreview />
    </template>
    <template #right>
      <ResultSetter />
    </template>
  </CommonTemplate>
</template>

<script>
import CommonTemplate from '../../components/CommonTemplate.vue'
import ResultCatalog from '../../modules/settingModule/result/CatalogPanel.vue'
import ResultPreview from '../../modules/settingModule/result/PreviewPanel.vue'
import ResultSetter from '../../modules/settingModule/result/SetterPanel.vue'

export default {
  name: 'ResultPage',
  components: {
    CommonTemplate,
    ResultCatalog,
    ResultPreview,
    ResultSetter
  }
}
</script>
