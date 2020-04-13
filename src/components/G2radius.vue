<template>
  <div>
    <v-card>
      <p id="mountNode" />
      <v-card-actions>
        <v-btn @click="$emit('goback')">返回</v-btn>
        <v-btn color="green lighten-2" dark @click="dialog = true">
          编辑名字
        </v-btn>
      </v-card-actions>
    </v-card>
    <v-dialog v-model="dialog" width="500">
      <v-card>
        <v-card-text>
          <v-text-field label="输入名字" v-model="pname"></v-text-field>
        </v-card-text>
        <v-divider></v-divider>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
            color="primary"
            flat
            @click="
              dialog = false;
              drawChart();
            "
          >
            ok
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
import G2 from "@antv/g2";
import DataSet from "@antv/data-set";
export default {
  data() {
    return {
      chart: null,
      dialog: false,
      pname: "你",
      imageColor: {
        天海春香: "#e22b30",
        如月千早: "#2743d2",
        萩原雪歩: "#2743d2",
        高槻やよい: "#f39939",
        秋月律子: "#01a860",
        三浦あずさ: "#9238be",
        水瀬伊織: "#fd99e1",
        菊地真: "#515558",
        "双海亜美/真美": "#ffe43f",
        星井美希: "#b4e04b",
        我那覇響: "#01adb9",
        四条貴音: "#a6126a",
        伊吹翼: "#fed552",
        エミリー: "#554171",
        大神環: "#ee762e",
        春日未来: "#ea5b76",
        北上麗花: "#6bb6b0",
        北沢志保: "#afa690",
        木下ひなた: "#d1342c",
        高坂海美: "#e9739b",
        佐竹美奈子: "#58a6dc",
        篠宮可憐: "#b63b40",
        島原エレナ: "#9bce92",
        ジュリア: "#d7385f",
        周防桃子: "#efb864",
        高山紗代子: "#7f6575",
        田中琴葉: "#92cfbb",
        天空橋朋花: "#bee3e3",
        徳川まつり: "#5abfb7",
        所恵美: "#454341",
        豊川風花: "#7278a8",
        中谷育: "#f7e78e",
        永吉昴: "#aeb49c",
        七尾百合子: "#c7b83c",
        二階堂千鶴: "#f19557",
        野々原茜: "#eb613f",
        箱崎星梨花: "#ed90ba",
        馬場このみ: "#f1becb",
        福田のり子: "#eceb70",
        舞浜歩: "#e25a9b",
        真壁瑞希: "#99b7dc",
        松田亜利沙: "#b54461",
        宮尾美也: "#b54461",
        最上静香: "#6495cf",
        望月杏奈: "#7e6ca8",
        百瀬莉緒: "#f19591",
        矢吹可奈: "#f5ad3b",
        横山奈緒: "#788bc5",
        ロコ: "#fff03c",
        桜守歌織: "#274079",
        白石紬: "#ebe1ff",
        櫻木真乃: "#ffbad9",
        風野灯織: "#1a4887",
        八宮めぐる: "#ffe011",
        月岡恋鐘: "#f94aad",
        田中摩美々: "#a745fb",
        白瀬咲耶: "#005d44",
        三峰結華: "#3990c4",
        幽谷霧子: "#d9f2ff",
        小宮果穂: "#e9471c",
        園田智代子: "#f9348c",
        西城樹里: "#ffc903",
        杜野凛世: "#89c4ed",
        有栖川夏葉: "#90e667",
        大崎甘奈: "#f54074",
        大崎甜花: "#e759ec",
        桑山千雪: "#fbfbfb",
        芹沢あさひ: "#f30100",
        黛冬優子: "#5ce626",
        和泉愛依: "#ff00ff",
        市川雛菜: "#f9cf49",
        浅倉透: "#65c4c6",
        樋口円香: "#c8164a",
        福丸小糸: "#7265ab",
        其他: "#ccc"
      }
    };
  },
  props: ["idols"],

  mounted() {
    setTimeout(() => {
      this.drawChart();
      this.loading = false;
    }, 10);
  },
  methods: {
    drawChart() {
      this.dv = new DataSet.DataView();
      this.dv.source(this.idols).transform({
        type: "percent",
        field: "c",
        dimension: "name",
        as: "percent"
      });
      if (this.chart) {
        // console.log("销毁旧图");
        this.chart.destroy();
      }
      // console.log("开始画图");
      this.chart = new G2.Chart({
        container: "mountNode",
        forceFit: true,
        padding: "auto",
        height: "600"
      });

      this.chart.source(this.dv);
      // 重要：绘制饼图时，必须声明 theta 坐标系
      this.chart.coord("theta", {
        radius: 0.65, // 设置饼图的大小
        innerRadius: 0.6,
        startAngle: (2 * Math.PI) / 2,
        endAngle: (6 * Math.PI) / 2
      });
      this.chart.tooltip({
        showTitle: false
      });
      this.chart
        .intervalStack()
        .position("percent")
        .color("name", name => this.imageColor[name])
        .tooltip("name*percent", (name, percent) => {
          return {
            name,
            value: (percent * 100).toFixed(1) + "%"
          };
        })
        .label("name", {
          textStyle: {
            fontSize: "13",
            fontWeight: "bold"
          },
          formatter: (text, item) => {
            const point = item.point; // 每个弧度对应的点
            let percent = point["percent"];
            percent = (percent * 100).toFixed(1) + "%";
            return text + " " + percent;
          }
        })
        .style({
          lineWidth: 1,
          stroke: "#eee"
        });
      this.chart.guide().html({
        position: ["50%", "50%"],
        html: `<div style="color:#878242;font-size: 20px;text-align: center;width: 10em;">
        ${this.pname}<br>
            <span style="color:#a39d4b;font-size:12px">最喜欢的偶像们</span></div>`,
        alignX: "middle",
        alignY: "middle"
      });
      this.chart.legend(true);
      this.chart.render();
    }
  }
};
</script>
