<script lang="ts" setup>
import { get } from '@vueuse/core';
import { type Plan } from '~/types';
import { getPlanSelectionName } from '~/utils/plans';

const props = withDefaults(
  defineProps<{
    plan: Plan;
    selected: boolean;
    popular?: boolean;
  }>(),
  { popular: false },
);

const emit = defineEmits<{ (e: 'click'): void }>();

const { t } = useI18n();
const css = useCssModule();

const { plan } = toRefs(props);

const name = computed(() => getPlanSelectionName(get(plan).months));
const totalPrice = computed(() => get(plan).priceFiat);
const price = computed(() => {
  const { months, priceFiat } = get(plan);
  return (parseFloat(priceFiat) / months).toFixed(2);
});

const click = () => {
  emit('click');
};
</script>

<template>
  <div :class="[css.plan, { [css.selected]: selected }]" @click="click()">
    <div class="flex items-center h-0 justify-center relative w-full">
      <RuiChip
        v-if="popular"
        :label="t('home.plans.most_popular')"
        class="-top-[2.9rem] absolute"
        color="primary"
        size="sm"
      />
    </div>
    <CheckMark :selected="selected" />
    <div :class="css.name">{{ name }}</div>
    <div :class="css.emphasis">{{ price }}€</div>
    <div :class="css.monthly">{{ t('home.plans.per_month') }}</div>
    <div :class="css.total">
      {{ t('home.plans.total', { total: totalPrice }) }}
    </div>
    <RuiButton :color="selected ? 'primary' : undefined" class="w-full">
      {{ t('home.plans.choose') }}
    </RuiButton>

    <div v-if="plan.discount" :class="css.discount">
      <RuiIcon class="text-black/60" name="hand-coin-line" />
      <span>
        {{ t('home.plans.save_discount', { discount: plan.discount }) }}
      </span>
    </div>
  </div>
</template>

<style lang="scss" module>
.plan {
  @apply flex flex-col items-center min-w-[14.5rem] xl:min-w-[13rem] 2xl:min-w-[13.5rem] w-full h-full px-6 py-8;
  @apply border border-solid rounded-lg cursor-pointer bg-white hover:bg-rui-primary/[0.01] border-black/[0.12];

  &.selected {
    @apply border-rui-primary;
  }
}

.name {
  @apply text-h5 text-rui-text mb-4;
}

.emphasis {
  @apply font-black text-h3 text-rui-text;
}

.monthly {
  @apply text-body-1 text-rui-secondary mt-4;
}

.total {
  @apply mb-4 sm:mb-8 text-body-1 text-rui-primary;
}

.discount {
  @apply flex gap-[0.62rem] text-base mt-2;
}
</style>
