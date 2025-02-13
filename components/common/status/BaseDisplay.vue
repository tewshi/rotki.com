<script setup lang="ts">
defineProps<{
  status: Status;
  message: string;
  title: string;
}>();

const statuses = ['success', 'error', 'neutral'] as const;

type Status = (typeof statuses)[number];

const css = useCssModule();
</script>

<template>
  <div :class="css.container">
    <div :class="css.wrapper">
      <div
        :class="{
          [css.header]: true,
          [css.error]: status === 'error',
          [css.success]: status === 'success',
          [css.neutral]: status === 'neutral',
        }"
      >
        <slot name="icon" />
      </div>
      <div :class="css.body">
        <div :class="css.title">{{ title }}</div>
        <div :class="css.message">{{ message }}</div>
        <slot />
      </div>
    </div>
  </div>
</template>

<style lang="scss" module>
.header {
  @apply shadow-lg rounded-t flex flex-row items-center justify-center;

  height: 120px;
}

.header.error {
  @apply bg-red-400;
}

.header.success {
  @apply bg-green-400;
}

.header.neutral {
  @apply bg-blue-400;
}

.wrapper {
  @apply border rounded mt-4;

  @media only screen and (max-width: 600px) {
    width: 100%;
  }

  @media only screen and (min-width: 601px) {
    width: 600px;
  }
}

.container {
  @apply w-full flex flex-row justify-center;
}

.body {
  @apply p-4 bg-white rounded-b shadow-lg;
}

.title {
  @apply font-bold flex flex-row justify-center mb-4 text-2xl leading-7;

  letter-spacing: 0;
}

.message {
  @apply break-words leading-5 text-lg font-medium;
}
</style>
