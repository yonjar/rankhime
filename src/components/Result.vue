<template>
  <v-card>
    <v-card-text class="title" v-if="sorted.length == 0">
        So Sad,看来您没有特别喜欢的偶像
    </v-card-text>
    <v-card-text class="title" v-else>
        你分配出来的爱
    </v-card-text>
    <v-container
      grid-list-md
      text-xs-center
    >
      <v-layout
        row
        wrap
      >
        <v-flex
          v-for="(item, index) in sorted"
          :key="index"
          :xs12="item.c == 100"
          :xs6="item.c <100 && item.c >=50"
          :xs3="item.c<50"
        >
          <v-card
            dark
            :color="`pink lighten-${[0,1,2,3,4,5][Math.floor((100-item.c)/20)]}`"
          >
            <v-card-text class="px-0"> <span class="text"> {{item.name}}{{item.c}}</span> </v-card-text>
          </v-card>
        </v-flex>
      </v-layout>

    </v-container>
     <v-divider></v-divider>
    <v-card-actions>
      <v-spacer />
      <v-btn
        @click="$emit('goback')"
      >返回编辑</v-btn>
      <v-btn
        @click="$emit('see',sorted)"
        color="success"
      >看看表</v-btn>
      <v-spacer />
    </v-card-actions>
  </v-card>
</template>
<script>
export default {
  data: () => ({
    sorted: []
  }),
  props: ["list"],
  mounted() {
    this.sorted = this.list
      .filter(el => el.c > 10)
      .sort((a, b) => {
        return b.c - a.c;
      });
  },
  methods: {
    style(c) {
      function width() {
        if (c == 100) return "100%";
        if (c >= 50) return "50%";
        return "25%";
      }
      return {
        color: "#fff",
        backgroundColor: `rgba(247, 65, 65, ${c / 100})`,
        width: `${width()}`
      };
    }
  }
};
</script>
<style scoped>
.idol {
  padding: 0;
  margin: 0;
  white-space: nowrap;
}
</style>
