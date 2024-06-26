<template>
  <div class="condition-wrapper" data-content-before="且">
    <span class="desc">如果</span>
    <el-form-item
      :prop="`conditions[${index}].field`"
      :rules="[{ required: true, message: '请选择题目', trigger: 'change' }]"
    >
      <el-select
        class="select field-select"
        v-model="conditionField"
        placeholder="请选择题目"
        @change="(val: any) => handleChange(conditionNode, 'field', val)"
      >
        <el-option v-for="{ label, value } in fieldList" :key="value" :label="label" :value="value">
        </el-option>
      </el-select>
    </el-form-item>
    <span class="desc">选择了</span>
    <el-form-item
      class="select value-select"
      :prop="`conditions[${index}].value`"
      :rules="[{ required: true, message: '请选择选项', trigger: 'change' }]"
    >
      <el-select
        v-model="conditionValue"
        placeholder="请选择选项"
        multiple
        @change="(val: any) => handleChange(conditionNode, 'value', val)"
      >
        <el-option
          v-for="{ label, value } in getRelyOptions"
          :key="value"
          :label="label"
          :value="value"
        >
        </el-option>
      </el-select>
    </el-form-item>
    <span class="desc">中的任一选项 </span>
    <span class="opt">
      <i-ep-plus class="opt-icon opt-icon-plus" @click="handleAdd" />
      <i-ep-minus
        class="opt-icon"
        v-if="index > 0"
        :size="14"
        @click="() => handleDelete(conditionNode.id)"
      />
    </span>
  </div>
</template>
<script setup lang="ts">
import { defineProps, computed, inject, ref, type ComputedRef } from 'vue'
import { ConditionNode, RuleNode } from '@/common/logicEngine/RuleBuild'
import { qAbleList } from '@/management/utils/constant.js'
import { cleanRichText } from '@/common/xss'
const renderData = inject<ComputedRef<Array<any>>>('renderData') || ref([])
const props = defineProps({
  index: {
    type: Number,
    default: 0
  },
  ruleNode: {
    type: RuleNode,
    default: () => {
      return {}
    }
  },
  conditionNode: {
    type: ConditionNode,
    default: () => {
      return {
        field: '',
        operator: '',
        value: ''
      }
    }
  }
})
const fieldList = computed(() => {
  const currentIndex = renderData.value.findIndex((item) => item.field === props.ruleNode.target)
  return renderData.value
    .slice(0, currentIndex)
    .filter((question: any) => qAbleList.includes(question.type))
    .map((item: any) => {
      return {
        label: `${item.showIndex ? item.indexNumber + '.' : ''} ${cleanRichText(item.title)}`,
        value: item.field
      }
    })
})
const getRelyOptions = computed(() => {
  const { field } = props.conditionNode
  if (!field) {
    return []
  }
  const currentQuestion = renderData.value.find((item) => item.field === field)
  return (
    currentQuestion?.options.map((item: any) => {
      return {
        label: cleanRichText(item.text),
        value: item.hash
      }
    }) || []
  )
})

const conditionField = computed(() => {
  return props.conditionNode.field
})

const conditionValue = computed(() => {
  return props.conditionNode.value
})

const handleChange = (conditionNode: ConditionNode, key: string, value: any) => {
  switch (key) {
    case 'field':
      conditionNode.setField(value)
      // 前置题改变清空选项
      conditionNode.setValue([])
      break
    case 'operator':
      conditionNode.setOperator(value)
      break
    case 'value':
      conditionNode.setValue(value)
      break
  }
}
const handleAdd = () => {
  props.ruleNode.addCondition(new ConditionNode())
}
const emit = defineEmits(['delete'])
const handleDelete = (id: any) => {
  emit('delete', id)
}
</script>
<style lang="scss" scoped>
.condition-wrapper {
  width: 100%;
  position: relative;
  display: flex;
  padding: 24px 0;
  &:not(:last-child)::before {
    content: attr(data-content-before);
    bottom: 0px;
    width: 20px;
    height: 20px;
    background: #fef6e6;
    border-radius: 2px;
    color: #faa600;
    font-size: 12px;
    display: flex;
    justify-content: center;
    align-items: center;
    position: absolute;
    bottom: -8px;
  }
  &:not(:last-child)::after {
    content: '';
    display: block;
    width: calc(100% - 32px);
    border-top: 1px dashed #e3e4e8;
    position: absolute;
    left: 32px;
    bottom: 0;
  }
  .desc {
    display: inline-block;
    margin-right: 12px;
    color: #333;
    line-height: 32px;
  }
  .opt {
    display: flex;
    align-items: center;
    .opt-icon {
      cursor: pointer;
      font-size: 12px;
    }

    .opt-icon-plus {
      margin-right: 10px;
    }
  }
  .el-form-item {
    display: inline-block;
    vertical-align: top !important;
    margin-right: 12px;
    margin-bottom: 0px;
  }
}
.select {
  width: 200px;
}
</style>
