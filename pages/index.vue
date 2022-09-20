<template>
  <Introduction />
</template>

<script lang="ts">
import Vue from 'vue'
import Introduction from '~/components/Introduction.vue';

export default Vue.extend({
    name: "IndexPage",
    components: { Introduction }
})
</script>
