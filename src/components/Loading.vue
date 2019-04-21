<template>
<v-card class="pa-5">    
  <v-progress-circular
    :size="100"
    :width="7"
    color="pink"
    indeterminate
  ></v-progress-circular>
</v-card>
</template>
<script>
export default {
    name:'loading'
}
</script>