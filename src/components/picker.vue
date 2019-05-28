<template>
  <div>
    <van-popup v-model="popupVisible" @open="onOpen" :position="position" :overlay="overlay">
      <van-picker
        :show-toolbar="true"
        :title="pickerTitle"
        :columns="pickerResult.columns"
        @change="onChange"
        @confirm="onConfirm"
      />
    </van-popup>
  </div>
</template>
<script>
import {
  getArrayByNum,
  getCurrTime,
  transToTimestamp,
  dateFormat,
  getWeekDay
} from "./util";

const today = dateFormat(new Date().getTime());

export default {
  props: {
    visible: {
      type: Boolean,
      default: false
    },
    startTime: {
      type: String,
      default: today,
      validator: value => {
        if( value === '') {
          return today;
        }
        const timeReg = /^\d{4}-\d{2}-\d{2} \d{2}:\d{2}:\d{2}$/;
        if (!timeReg.test(value)) {
          throw `startTime 格式不正确：${value}`;
        }
        return value;
      },
    },
    during: {
      // 时长: 90 天
      type: Number,
      default: 90
    },
    interval: {
      // 时间间隔：15 分钟
      type: [String, Number],
      default: 15
    },
    position: {
      // 位置，可选值为 top bottom right left
      type: String,
      default: "bottom"
    },
    overlay: {
      // 	是否显示遮罩层
      type: Boolean,
      default: true
    }
  },
  computed: {
    popupVisible: {
      set() {
        this.handleCancel();
      },
      get() {
        return this.visible;
      }
    }
  },
  data() {
    return {
      pickerTitle: "取车时间",

      isToday: false,
      currTime: {},

      pickerResult: {
        values: [],
        indexList: [0, 0],
        columns: [
          {
            values: [],
            className: "column1"
          },
          {
            values: [],
            className: "column2"
          }
        ]
      }
    };
  },

  methods: {
    getArrayByNum,
    getCurrTime,
    transToTimestamp,
    dateFormat,
    getWeekDay,

    // 设置日期和星期几
    // 日期范围：从 startTime/今天 到90天后截止
    // 注意处理 90 天内跨年的情况
    setColumnDays() {

      const currTimestamp = this.transToTimestamp(this.startTime);
      const secondInOneDay = 24 * 60 * 60 * 1000;
      const during = this.during;

      const columnDays = [].map.call(
        [...Array(during).keys()],
        (item, index) => {
          const timestamp = currTimestamp + secondInOneDay * index;
          return (
            this.dateFormat(timestamp, "MM月DD日") +
            "" +
            this.getWeekDay(timestamp)
          );
        }
      );

      if (this.isToday) {
        columnDays[0] = "今天";
      }
      this.pickerResult.columns[0].values = columnDays;
    },
    // 设置小时和分钟
    setColumnHours() {
      const { indexList, columns } = this.pickerResult;
      const { hh, mm } = this.currTime;
      const hours = getArrayByNum(24);
      const minutes = getArrayByNum(60, this.interval);
      const newColumns = JSON.parse(JSON.stringify(columns));
      const list = [];

      // 小时
      let hourList = [];
      if (this.isToday && indexList[0] === 0) {
        // 选择今天
        hourList = hours.filter(h => h >= hh);
      } else {
        hourList = hours;
      }

      hourList.forEach((item, index) => {
        if (this.isToday && indexList[0] === 0 && index === 0) {
          // 今天，而且是当前小时内
          minutes
            .filter(m => m >= mm)
            .forEach(m => {
              list.push(item + ":" + m);
            });
        } else {
          minutes.forEach(m => {
            list.push(item + ":" + m);
          });
        }
      });

      newColumns[1].values = list;

      this.pickerResult.columns = newColumns;

      this.pickerResult.values[1] = list[0];
      this.setPickertitle(this.pickerResult.values[1]);
    },
    showPicker() {
      this.initPickerData();
    },

    initPickerData() {
      this.setColumnDays();
      this.setColumnHours();

      // 设置 title 初始值
      const values = this.pickerResult.columns.map(item => item.values[0]);
      this.setPickertitle(values);
    },

    setPickertitle(values) {
      const [day, hour] = values;
      const { YYYY } = this.currTime;
      // 文字太长，可以考虑把星期几去掉
      // this.pickerTitle = (YYYY + "年" + day + " " + hour).replace(/星期.{1}/, '');
      this.pickerTitle = YYYY + "年" + day + " " + hour;
    },

    getPickerIndex(picker, values, index) {
      const indexList = picker.columns
        .map(item => item.values)
        .map((list, index) => list.indexOf(values[index]));

      this.pickerResult.indexList = indexList;
      this.setColumnHours();
    },

    // change 事件 -> 设置 title -> 如果是"今天",就会修改数据,但是不会触发 change 事件,此时title没有更新,需要再次手动设置
    onChange(picker, values, index) {
      this.pickerResult.values = values;
      this.getPickerIndex.apply(this, arguments);
      this.setPickertitle(values);
    },

    handleStartTime () {
      const date = this.startTime.split(/\s/)[0]
      const today = this.dateFormat();

      this.isToday = today.includes(date);
    },

    onOpen() {
      this.currTime = this.getCurrTime();
      this.handleStartTime();
      this.showPicker();
    },

    // 点击确定
    onConfirm(values) {
      const { YYYY, MM, DD, hh, mm, ss } = this.currTime;
      const str = values[0];
      let result, time, timestamp;
      console.log(this.currTime)
      if (str === "今天") {
        time = [YYYY, MM, DD].join("-") + " " + values[1] + ":00";
      } else {
        result = str.match(/\d+/g);
        if (result[0] < MM) {
          // 选择的月份小于当前月份，可判断为明年
          result.unshift(YYYY + 1);
        } else {
          result.unshift(YYYY);
        }
        time = result.join("-") + " " + values[1] + ":00";
      }

      timestamp = this.transToTimestamp(time);

      this.handleCancel();

      this.$emit("confirm", time, timestamp);
    },

    handleCancel() {
      this.$emit("update:visible", false);
    }
  }
};
</script>

<style>
.van-picker-column__item {
  transition: font-size 0.3s;
}
.van-picker-column__item--selected {
  font-size: 120%;
}
</style>
