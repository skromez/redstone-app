<template>
  <div class="stat-elem">
    <div class="stat-title">
      {{ title }}:&nbsp;
    </div>
    <div class="stat-value" v-if="isCustomUrl">
      {{ value }}
    </div>
    <div class="stat-value" v-else>
      {{ value | price }}
    </div>
  </div>
</template>

<script>
export default {
  name: 'StatElem',
  props: {
    value: String,
    title: String,
    isCustomUrl: Boolean,
  },

}
</script>

<style scoped lang="scss">
@import '~@/styles/app';

.stat-elem {
  color: $gray-750;
  display: flex;

  .stat-title {
    font-size: 14px;
    font-weight: $font-weight-ultra-thin;
  }

  .stat-value {
    font-size: 14px;
    font-weight: $font-weight-soft-bold;
  }
}
</style>
