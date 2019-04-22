<template>
  <v-app>
    <v-toolbar app>
      <v-toolbar-title class="headline text-uppercase">
        <span>SHINY COLORS </span>
        <span class="font-weight-light">DD test</span>
      </v-toolbar-title>
      <v-spacer></v-spacer>
    </v-toolbar>
    <v-content>
      <div v-if="step==0">
        <v-container>

          <v-card class="pt-0">
            <v-progress-linear
              background-color="pink lighten-3"
              color="pink lighten-1"
              :value="group*(100/i)"
            ></v-progress-linear>
            <div class="px-3 mt-5">
              <div
                v-for="(item, index) in crrt"
                :key="index"
              >
                <v-slider
                  v-model="item.c"
                  color="pink"
                  thumb-color="pink lighten-3"
                  thumb-label
                >
                  <template v-slot:prepend>
                    <div style="width:90px;">{{item.name}}</div>
                  </template>
                  <template v-slot:append>
                    <v-icon
                      color="pink"
                      @click="item.c == 100?(item.c=0):(item.c=100)"
                    >
                      {{item.c ==100?'favorite':'favorite_border'}}
                    </v-icon>
                  </template>
                </v-slider>
              </div>
            </div>
            <v-divider></v-divider>
            <v-card-actions>
              <v-btn
                @click="reset"
                color="error"
              >重置所有</v-btn>
              <v-spacer />
              <v-btn
                @click="group-=6"
                color="success"
                :disabled="group==0"
              >上一组</v-btn>
              <v-btn
                @click="nextGroup"
                color="success"
              >{{group>=i?"完成":"下一组"}}</v-btn>
            </v-card-actions>
          </v-card>
        </v-container>
      </div>
      <div v-if="step == 1">
        <result
          :list="list"
          @goback="step = 0"
          @see="step=2;sorted=$event"
        />
      </div>
      <div v-if="step == 2">
        <G2radius
          :idols="sorted"
          @goback="step = 0"
        />
       
      </div>
    </v-content>
  </v-app>
</template>

<script>
import Loading from "@/components/Loading.vue";
export default {
  name: "App",
  data() {
    return {
      list: [],
      group: 0,
      crrt: [],
      btnText: "下一组",
      step: 0,
      sorted: [],
      i: 0
    };
  },
  mounted() {
    require("@/assets/idol.json")['283'].forEach((el, i) => {
      this.list.push({
        name: el,
        id: i,
        c: 0
      });
    });
    this.crrt = this.list.slice(this.group, this.group + 6);
    this.i = Math.floor((this.list.length-1)/6) * 6;
  },
  watch: {
    group() {
      this.crrt = this.list.slice(this.group, this.group + 6);
    }
  },
  methods: {
    nextGroup() {
      if (this.group >= this.i) {
        this.step = 1;
      } else {
        this.group += 6;
      }
    },
    reset() {
      for (let item of this.list) {
        item.c = 0;
      }
    }
  },
  components: {
    
    result: () => import("@/components/Result.vue"),
    G2radius:() => ({
      // 需要加载的组件 (应该是一个 `Promise` 对象)
      component: import("@/components/G2radius.vue"),
      // 异步组件加载时使用的组件
      loading: Loading,
      // 加载失败时使用的组件
      // error: ErrorComponent,
      // 展示加载时组件的延时时间。默认值是 200 (毫秒)
      // delay: 200,
      // 如果提供了超时时间且组件加载也超时了，
      // 则使用加载失败时使用的组件。默认值是：`Infinity`
      // timeout: 3000
    })
  }
};
</script>
