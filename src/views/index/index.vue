<template>
  <div flex-col-center wh-full>
    <div class="w-500px">
      <n-h1 class="text-center">{{ title }}</n-h1>
      <n-form ref="formRef" :model="model">
        <n-form-item path="platform" label="平台名称">
          <n-select
            v-model:value="model.platform"
            filterable
            placeholder="请选择平台名称"
            :options="config"
            label-field="name"
            value-field="name"
            @update:value="changePlatform"
          />
        </n-form-item>
        <n-form-item path="amount" label="欠款金额">
          <n-input-number v-model:value="model.amount" class="w-full" placeholder="请输入欠款金额" />
        </n-form-item>
        <n-button :disabled="!model.platform || !model.amount" type="primary" block @click="createPlanDetail">
          一键智能债务规划
        </n-button>
      </n-form>

      <n-descriptions v-if="!!resultDetail" label-placement="left" bordered :column="1" class="mt-xl">
        <n-descriptions-item label="平台名称">{{ resultDetail.name }}</n-descriptions-item>
        <n-descriptions-item label="期数（分/延）">{{ resultDetail.num }}</n-descriptions-item>
        <n-descriptions-item label="每期需还">{{ resultDetail.money }}</n-descriptions-item>
        <n-descriptions-item label="减免政策">{{ resultDetail.policy }}</n-descriptions-item>
      </n-descriptions>

      <n-p>
        <n-h3>温馨提示：</n-h3>
        如果您欠款金额较高，建议及时还款，如有难度，可提前做好协商规划，避免起诉执行
      </n-p>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import { FormInst, FormRules } from 'naive-ui';
import { config } from './config';

const title = import.meta.env.VITE_APP_TITLE;

const currentPlatform = ref();
function changePlatform(value: any, option: any) {
  currentPlatform.value = option;
}
function getPlan(value: number, intervals: any) {
  for (const interval of Object.keys(intervals)) {
    const [start, end] = interval.split(',');
    if (end == 'end') {
      return intervals[interval];
    }
    if (value >= parseFloat(start) && value <= parseFloat(end)) {
      return intervals[interval];
    }
  }
  return null;
}

const resultDetail = ref();
function createPlanDetail() {
  const { amount } = model.value;
  if (!amount) return;

  const { name, policy, plan, noPlan } = currentPlatform.value;

  let num = getPlan(amount, plan);
  let money = (amount / num).toFixed(2);

  if (noPlan) {
    num = '--';
    money = '--';
  }
  const result = { name, num, money, policy };

  resultDetail.value = result;
}

const formRef = ref<FormInst | null>(null);

interface ModelType {
  platform: string | null;
  amount: number | null;
}
const model = ref<ModelType>({
  platform: null,
  amount: null,
});
</script>

<style lang="less" scoped></style>
