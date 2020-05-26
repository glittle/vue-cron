<template>
  <div class="VueCron">
    <el-tabs type="border-card">
      <el-tab-pane>
        <span slot="label">
          <i class="el-icon-info"></i>
          {{text.Samples.name}}
        </span>
        <div class="tabBody samples">
          <el-row>
            <button v-on:click="use(1)">Use</button>
            <span ref="s1" data-cron="0 0 10 ? * TUE *">Every Tuesday at 10 am</span>
          </el-row>
          <el-row>
            <button v-on:click="use(2)">Use</button>
            <span ref="s2" data-cron="0 0 2 21 * ? *">Every month on the 21st (at 2 am)</span>
          </el-row>
          <el-row>
            <button v-on:click="use(3)">Use</button>
            <span ref="s3" data-cron="0 0 0 1 0/3 ? *">First day of every quarter</span>
          </el-row>
          <el-row>
            <button v-on:click="use(4)">Use</button>
            <span ref="s4" data-cron="0 0 0 1W 10 ? *">Every year on the first weekday in October</span>
          </el-row>
        </div>
      </el-tab-pane>
      <el-tab-pane>
        <span slot="label">
          <i class="el-icon-date"></i>
          {{text.Year.name}}
        </span>
        <div class="tabBody">
          <el-row>
            <el-radio v-model="year.cronEvery" label="1">
              {{text.Year.every}}
              <span></span>
            </el-radio>
          </el-row>
          <el-row>
            <el-radio v-model="year.cronEvery" label="2">
              {{text.Year.interval[0]}}
              <el-input-number size="small" v-model="year.incrementIncrement" :min="1" :max="99"></el-input-number>
              {{text.Year.interval[1]}}
              <el-input-number size="small" v-model="year.incrementStart" :min="2000" :max="2200"></el-input-number>
            </el-radio>
          </el-row>
          <el-row>
            <el-radio class="long" v-model="year.cronEvery" label="3">
              {{text.Year.specific}}
              <el-select size="small" filterable multiple v-model="year.specificSpecific">
                <el-option v-for="val in 100" :key="val" :label="2019+val" :value="2019+val"></el-option>
              </el-select>
            </el-radio>
          </el-row>
          <el-row>
            <el-radio v-model="year.cronEvery" label="4">
              {{text.Year.cycle[0]}}
              <el-input-number size="small" v-model="year.rangeStart" :min="2000" :max="2200"></el-input-number>
              {{text.Year.cycle[1]}}
              <el-input-number size="small" v-model="year.rangeEnd" :min="2000" :max="2200"></el-input-number>
            </el-radio>
          </el-row>
        </div>
      </el-tab-pane>
      <el-tab-pane>
        <span slot="label">
          <i class="el-icon-date"></i>
          {{text.Month.name}}
        </span>
        <div class="tabBody">
          <el-row>
            <el-radio v-model="month.cronEvery" label="1">
              {{text.Month.every}}
              <span></span>
            </el-radio>
          </el-row>
          <el-row>
            <el-radio v-model="month.cronEvery" label="2">
              {{text.Month.interval[0]}}
              <el-input-number size="small" v-model="month.incrementIncrement" :min="0" :max="12"></el-input-number>
              {{text.Month.interval[1]}}
              <el-input-number size="small" v-model="month.incrementStart" :min="0" :max="12"></el-input-number>
            </el-radio>
          </el-row>
          <el-row>
            <el-radio class="long" v-model="month.cronEvery" label="3">
              {{text.Month.specific}}
              <el-select size="small" multiple v-model="month.specificSpecific">
                <el-option v-for="val in 12" :key="val" :label="val" :value="val"></el-option>
              </el-select>
            </el-radio>
          </el-row>
          <el-row>
            <el-radio v-model="month.cronEvery" label="4">
              {{text.Month.cycle[0]}}
              <el-input-number size="small" v-model="month.rangeStart" :min="1" :max="12"></el-input-number>
              {{text.Month.cycle[1]}}
              <el-input-number size="small" v-model="month.rangeEnd" :min="1" :max="12"></el-input-number>
            </el-radio>
          </el-row>
        </div>
      </el-tab-pane>
      <el-tab-pane>
        <span slot="label">
          <i class="el-icon-date"></i>
          {{text.Day.name}}
        </span>
        <div class="tabBody">
          <el-row>
            <el-radio v-model="day.cronEvery" label="1">
              {{text.Day.every}}
              <span></span>
            </el-radio>
          </el-row>
          <el-row>
            <el-radio v-model="day.cronEvery" label="3">
              {{text.Day.intervalDay[0]}}
              <el-input-number size="small" v-model="day.incrementIncrement" :min="1" :max="31"></el-input-number>
              {{text.Day.intervalDay[1]}}
              <el-input-number size="small" v-model="day.incrementStart" :min="1" :max="31"></el-input-number>
              {{text.Day.intervalDay[2]}}
            </el-radio>
          </el-row>
          <el-row>
            <el-radio class="long" v-model="day.cronEvery" label="5">
              {{text.Day.specificDay}}
              <el-select size="small" multiple v-model="day.specificSpecific">
                <el-option v-for="val in 31" :key="val" :value="val">{{val}}</el-option>
              </el-select>
            </el-radio>
          </el-row>
          <el-row>
            <el-radio class="long" v-model="day.cronEvery" label="4">
              {{text.Day.specificWeek}}
              <el-select size="small" multiple v-model="week.specificSpecific">
                <el-option
                  v-for="val in 7"
                  :key="val"
                  :label="text.Week[val-1]"
                  :value="['SUN','MON','TUE','WED','THU','FRI','SAT'][val-1]"
                ></el-option>
              </el-select>
            </el-radio>
          </el-row>
          <el-row>
            <el-radio v-model="day.cronEvery" label="2">
              {{text.Day.intervalWeek[0]}}
              <el-input-number size="small" v-model="week.incrementIncrement" :min="1" :max="7"></el-input-number>
              {{text.Day.intervalWeek[1]}}
              <el-select size="small" v-model="week.incrementStart">
                <el-option v-for="val in 7" :key="val" :label="text.Week[val-1]" :value="val"></el-option>
              </el-select>
              {{text.Day.intervalWeek[2]}}
            </el-radio>
          </el-row>
          <el-row>
            <el-radio v-model="day.cronEvery" label="10">
              {{text.Day.nearestWeekday[0]}}
              <el-input-number size="small" v-model="day.cronDaysNearestWeekday" :min="1" :max="31"></el-input-number>
              {{text.Day.nearestWeekday[1]}}
            </el-radio>
          </el-row>
          <el-row>
            <el-radio v-model="day.cronEvery" label="11">
              {{text.Day.someWeekday[0]}}
              <el-input-number size="small" v-model="week.cronNthDayNth" :min="1" :max="5"></el-input-number>
              <el-select size="small" v-model="week.cronNthDayDay">
                <el-option v-for="val in 7" :key="val" :label="text.Week[val-1]" :value="val"></el-option>
              </el-select>
              {{text.Day.someWeekday[1]}}
            </el-radio>
          </el-row>
          <el-row>
            <el-radio v-model="day.cronEvery" label="8">
              {{text.Day.lastWeek[0]}}
              <el-select size="small" v-model="day.cronLastSpecificDomDay">
                <el-option v-for="val in 7" :key="val" :label="text.Week[val-1]" :value="val"></el-option>
              </el-select>
              {{text.Day.lastWeek[1]||''}}
            </el-radio>
          </el-row>
          <el-row>
            <el-radio v-model="day.cronEvery" label="9">
              <span></span>
              <el-input-number size="small" v-model="day.cronDaysBeforeEomMinus" :min="1" :max="31"></el-input-number>
              {{text.Day.beforeEndMonth[0]}}
            </el-radio>
          </el-row>
          <el-row>
            <el-radio v-model="day.cronEvery" label="7">
              {{text.Day.lastWeekday}}
              <span></span>
            </el-radio>
          </el-row>
          <el-row>
            <el-radio v-model="day.cronEvery" label="6">
              {{text.Day.lastDay}}
              <span></span>
            </el-radio>
          </el-row>
        </div>
      </el-tab-pane>
      <el-tab-pane v-if="showHours">
        <span slot="label">
          <i class="el-icon-time"></i>
          {{text.Hours.name}}
        </span>
        <div class="tabBody">
          <el-row>
            <el-radio v-model="hour.cronEvery" label="1">
              {{text.Hours.every}}
              <span></span>
            </el-radio>
          </el-row>
          <el-row>
            <el-radio v-model="hour.cronEvery" label="2">
              {{text.Hours.interval[0]}}
              <el-input-number size="small" v-model="hour.incrementIncrement" :min="0" :max="23"></el-input-number>
              {{text.Hours.interval[1]}}
              <el-input-number size="small" v-model="hour.incrementStart" :min="0" :max="23"></el-input-number>
              {{text.Hours.interval[2]}}
            </el-radio>
          </el-row>
          <el-row>
            <el-radio class="long" v-model="hour.cronEvery" label="3">
              {{text.Hours.specific}}
              <el-select size="small" multiple v-model="hour.specificSpecific">
                <el-option v-for="val in 24" :key="val" :value="val-1">{{val-1}}</el-option>
              </el-select>
            </el-radio>
          </el-row>
          <el-row>
            <el-radio v-model="hour.cronEvery" label="4">
              {{text.Hours.cycle[0]}}
              <el-input-number size="small" v-model="hour.rangeStart" :min="0" :max="23"></el-input-number>
              {{text.Hours.cycle[1]}}
              <el-input-number size="small" v-model="hour.rangeEnd" :min="0" :max="23"></el-input-number>
              {{text.Hours.cycle[2]}}
            </el-radio>
          </el-row>
        </div>
      </el-tab-pane>
      <el-tab-pane v-if="showMinutes">
        <span slot="label">
          <i class="el-icon-time"></i>
          {{text.Minutes.name}}
        </span>
        <div class="tabBody">
          <el-row>
            <el-radio v-model="minute.cronEvery" label="1">
              {{text.Minutes.every}}
              <span></span>
            </el-radio>
          </el-row>
          <el-row>
            <el-radio v-model="minute.cronEvery" label="2">
              {{text.Minutes.interval[0]}}
              <el-input-number size="small" v-model="minute.incrementIncrement" :min="1" :max="60"></el-input-number>
              {{text.Minutes.interval[1]}}
              <el-input-number size="small" v-model="minute.incrementStart" :min="0" :max="59"></el-input-number>
              {{text.Minutes.interval[2]||''}}
            </el-radio>
          </el-row>
          <el-row>
            <el-radio class="long" v-model="minute.cronEvery" label="3">
              {{text.Minutes.specific}}
              <el-select size="small" multiple v-model="minute.specificSpecific">
                <el-option v-for="val in 60" :key="val" :value="val-1">{{val-1}}</el-option>
              </el-select>
            </el-radio>
          </el-row>
          <el-row>
            <el-radio v-model="minute.cronEvery" label="4">
              {{text.Minutes.cycle[0]}}
              <el-input-number size="small" v-model="minute.rangeStart" :min="1" :max="60"></el-input-number>
              {{text.Minutes.cycle[1]}}
              <el-input-number size="small" v-model="minute.rangeEnd" :min="0" :max="59"></el-input-number>
              {{text.Minutes.cycle[2]}}
            </el-radio>
          </el-row>
        </div>
      </el-tab-pane>

      <el-tab-pane v-if="showSeconds">
        <span slot="label">
          <i class="el-icon-time"></i>
          {{text.Seconds.name}}
        </span>
        <div class="tabBody">
          <el-row>
            <el-radio v-model="second.cronEvery" label="1">
              {{text.Seconds.every}}
              <span></span>
            </el-radio>
          </el-row>
          <el-row>
            <el-radio v-model="second.cronEvery" label="2">
              {{text.Seconds.interval[0]}}
              <el-input-number size="small" v-model="second.incrementIncrement" :min="1" :max="60"></el-input-number>
              {{text.Seconds.interval[1]||''}}
              <el-input-number size="small" v-model="second.incrementStart" :min="0" :max="59"></el-input-number>
              {{text.Seconds.interval[2]||''}}
            </el-radio>
          </el-row>
          <el-row>
            <el-radio class="long" v-model="second.cronEvery" label="3">
              {{text.Seconds.specific}}
              <el-select size="small" multiple v-model="second.specificSpecific">
                <el-option v-for="val in 60" :key="val" :value="val-1">{{val-1}}</el-option>
              </el-select>
            </el-radio>
          </el-row>
          <el-row>
            <el-radio v-model="second.cronEvery" label="4">
              {{text.Seconds.cycle[0]}}
              <el-input-number size="small" v-model="second.rangeStart" :min="1" :max="60"></el-input-number>
              {{text.Seconds.cycle[1]||''}}
              <el-input-number size="small" v-model="second.rangeEnd" :min="0" :max="59"></el-input-number>
              {{text.Seconds.cycle[2]||''}}
            </el-radio>
          </el-row>
        </div>
      </el-tab-pane>
    </el-tabs>
    <div class="result">
      <span class="value">{{this.final}}</span>
      <el-button size="small" type="primary" @click="change">{{text.Save}}</el-button>
      <el-button size="small" @click="close">{{text.Cancel}}</el-button>
    </div>
  </div>
</template>
<script>
import Language from "./language/index";
export default {
  name: "VueCron",
  props: {
    value: String,
    showSeconds: {
      type: Boolean,
      default: false
    },
    showMinutes: {
      type: Boolean,
      default: false
    },
    showHours: {
      type: Boolean,
      default: true
    },
    lang: String
  },
  data() {
    return {
      second: {
        cronEvery: "",
        incrementStart: "3",
        incrementIncrement: "5",
        rangeStart: "",
        rangeEnd: "",
        specificSpecific: []
      },
      minute: {
        cronEvery: "",
        incrementStart: "3",
        incrementIncrement: "5",
        rangeStart: "",
        rangeEnd: "",
        specificSpecific: []
      },
      hour: {
        cronEvery: "",
        incrementStart: "3",
        incrementIncrement: "5",
        rangeStart: "",
        rangeEnd: "",
        specificSpecific: []
      },
      day: {
        cronEvery: "",
        incrementStart: "1",
        incrementIncrement: "1",
        rangeStart: "",
        rangeEnd: "",
        specificSpecific: [],
        cronLastSpecificDomDay: 1,
        cronDaysBeforeEomMinus: "",
        cronDaysNearestWeekday: ""
      },
      week: {
        cronEvery: "",
        incrementStart: "1",
        incrementIncrement: "1",
        specificSpecific: [],
        cronNthDayDay: 1,
        cronNthDayNth: "1"
      },
      month: {
        cronEvery: "",
        incrementStart: "3",
        incrementIncrement: "5",
        rangeStart: "",
        rangeEnd: "",
        specificSpecific: []
      },
      year: {
        cronEvery: "",
        incrementStart: "2020",
        incrementIncrement: "1",
        rangeStart: "",
        rangeEnd: "",
        specificSpecific: []
      }
    };
  },
  //   watch: {
  //     data() {
  //       this.rest(this.$data);
  //     }
  //   },
  computed: {
    text() {
      return Language[this.lang || "en"];
    },
    secondsText() {
      let seconds = "";
      let cronEvery = this.second.cronEvery;
      switch (cronEvery.toString()) {
        case "1":
          seconds = "*";
          break;
        case "2":
          seconds =
            this.second.incrementStart + "/" + this.second.incrementIncrement;
          break;
        case "3":
          this.second.specificSpecific.map(val => {
            seconds += val + ",";
          });
          seconds = seconds.slice(0, -1);
          break;
        case "4":
          seconds = this.second.rangeStart + "-" + this.second.rangeEnd;
          break;
      }
      return seconds;
    },
    minutesText() {
      let minutes = "";
      let cronEvery = this.minute.cronEvery;
      switch (cronEvery.toString()) {
        case "1":
          minutes = "*";
          break;
        case "2":
          minutes =
            this.minute.incrementStart + "/" + this.minute.incrementIncrement;
          break;
        case "3":
          this.minute.specificSpecific.map(val => {
            minutes += val + ",";
          });
          minutes = minutes.slice(0, -1);
          break;
        case "4":
          minutes = this.minute.rangeStart + "-" + this.minute.rangeEnd;
          break;
      }
      return minutes;
    },
    hoursText() {
      let hours = "";
      let cronEvery = this.hour.cronEvery;
      switch (cronEvery.toString()) {
        case "1":
          hours = "*";
          break;
        case "2":
          hours = this.hour.incrementStart + "/" + this.hour.incrementIncrement;
          break;
        case "3":
          this.hour.specificSpecific.map(val => {
            hours += val + ",";
          });
          hours = hours.slice(0, -1);
          break;
        case "4":
          hours = this.hour.rangeStart + "-" + this.hour.rangeEnd;
          break;
      }
      return hours;
    },
    daysText() {
      let days = "";
      let cronEvery = this.day.cronEvery;
      switch (cronEvery.toString()) {
        case "1":
          break;
        case "2":
        case "4":
        case "11":
          days = "?";
          break;
        case "3":
          days = this.day.incrementStart + "/" + this.day.incrementIncrement;
          break;
        case "5":
          this.day.specificSpecific.map(val => {
            days += val + ",";
          });
          days = days.slice(0, -1);
          break;
        case "6":
          days = "L";
          break;
        case "7":
          days = "LW";
          break;
        case "8":
          days = this.day.cronLastSpecificDomDay + "L";
          break;
        case "9":
          days = "L-" + this.day.cronDaysBeforeEomMinus;
          break;
        case "10":
          days = this.day.cronDaysNearestWeekday + "W";
          break;
      }
      return days;
    },
    weeksText() {
      let weeks = "";
      let cronEvery = this.day.cronEvery;
      switch (cronEvery.toString()) {
        case "1":
        case "3":
        case "5":
          weeks = "?";
          break;
        case "2":
          weeks = this.week.incrementStart + "/" + this.week.incrementIncrement;
          break;
        case "4":
          this.week.specificSpecific.map(val => {
            weeks += val + ",";
          });
          weeks = weeks.slice(0, -1);
          break;
        case "6":
        case "7":
        case "8":
        case "9":
        case "10":
          weeks = "?";
          break;
        case "11":
          weeks = this.week.cronNthDayDay + "#" + this.week.cronNthDayNth;
          break;
      }
      return weeks;
    },
    monthsText() {
      let months = "";
      let cronEvery = this.month.cronEvery;
      switch (cronEvery.toString()) {
        case "1":
          months = "*";
          break;
        case "2":
          months =
            this.month.incrementStart + "/" + this.month.incrementIncrement;
          break;
        case "3":
          this.month.specificSpecific.map(val => {
            months += val + ",";
          });
          months = months.slice(0, -1);
          break;
        case "4":
          months = this.month.rangeStart + "-" + this.month.rangeEnd;
          break;
      }
      return months;
    },
    yearsText() {
      let years = "";
      let target = this.year;
      let cronEvery = target.cronEvery;
      switch (cronEvery.toString()) {
        case "1":
          years = "*";
          break;
        case "2":
          years = target.incrementStart + "/" + target.incrementIncrement;
          break;
        case "3":
          target.specificSpecific.map(val => {
            years += val + ",";
          });
          years = years.slice(0, -1);
          break;
        case "4":
          years = target.rangeStart + "-" + target.rangeEnd;
          break;
      }
      return years;
    },
    final() {
      return `${this.secondsText || "*"} ${this.minutesText || "*"} ${this
        .hoursText || "*"} ${this.daysText || "*"} ${this.monthsText ||
        "*"} ${this.weeksText || "?"} ${this.yearsText || "*"}`;
    }
  },
  watch: {
    value(a) {
      this.parse(a);
    }
  },
  mounted() {
    this.parse(this.value);
  },
  methods: {
    change() {
      this.$emit("input", this.final);
      this.$emit("close");
    },
    close() {
      this.$emit("close");
    },
    use(sampleNum) {
      var ref = this.$refs["s" + sampleNum];
      if (ref) {
        this.parse(ref.dataset.cron);
      }
    },
    parse(s) {
      if (!s) return;

      // ref github.com/bradymholt/cron-expression-descriptor/blob/master/lib/ExpressionParser.cs
      var parts = s.split(" ");
      var numParts = parts.length;
      if (numParts < 5) return;
      if (numParts > 7) return;
      if (numParts === 6) {
        // has seconds or years
        if (+parts[numParts - 1].substr(0, 4) > 1000) {
          // years
          parts.unshift("*"); // add seconds
        } else {
          parts.push(""); // add years
        }
      } else if (numParts === 5) {
        // add seconds and years
        parts.unshift("*");
        parts.push("");
      }

      this.parseSecond(this.showSeconds ? parts[0] : "0");
      this.parseMinute(this.showMinutes ? parts[1] : "0");
      this.parseHour(this.showHours ? parts[2] : "0");
      this.parseDom(parts[3]);
      this.parseMonth(parts[4]);
      this.parseDow(parts[5]);
      this.parseYear(parts[6]);
    },
    parseSecond(s) {
      var target = this.second;
      if (s === "*") {
        target.cronEvery = "1";
        return;
      }
      var parts = s.split("/");
      if (parts.length === 2) {
        target.incrementStart = parts[0];
        target.incrementIncrement = parts[1];
        target.cronEvery = "2";
        return;
      }
      parts = s.split("-");
      if (parts.length > 1) {
        target.rangeStart = parts[0];
        target.rangeEnd = parts[1];
        target.cronEvery = "4";
        return;
      }
      parts = s.split(",");
      var num = parts[0];
      if (+num < 60) {
        target.specificSpecific = parts;
        target.cronEvery = "3";
        return;
      }
      // default
      target.cronEvery = "1";
    },
    parseMinute(s) {
      var target = this.minute;
      if (s === "*") {
        target.cronEvery = "1";
        return;
      }
      var parts = s.split("/");
      if (parts.length === 2) {
        target.incrementStart = parts[0];
        target.incrementIncrement = parts[1];
        target.cronEvery = "2";
        return;
      }
      parts = s.split("-");
      if (parts.length > 1) {
        target.rangeStart = parts[0];
        target.rangeEnd = parts[1];
        target.cronEvery = "4";
        return;
      }
      parts = s.split(",");
      var num = parts[0];
      if (+num < 60) {
        target.specificSpecific = parts;
        target.cronEvery = "3";
        return;
      }
      // default
      target.cronEvery = "1";
    },
    parseHour(s) {
      var target = this.hour;
      if (s === "*") {
        target.cronEvery = "1";
        return;
      }
      var parts = s.split("/");
      if (parts.length === 2) {
        target.incrementStart = parts[0];
        target.incrementIncrement = parts[1];
        target.cronEvery = "2";
        return;
      }
      parts = s.split("-");
      if (parts.length > 1) {
        target.rangeStart = parts[0];
        target.rangeEnd = parts[1];
        target.cronEvery = "4";
        return;
      }
      parts = s.split(",");
      var num = parts[0];
      if (+num < 24) {
        target.specificSpecific = parts;
        target.cronEvery = "3";
        return;
      }
      // default
      target.cronEvery = "1";
    },
    parseDom(s) {
      var target = this.day;
      if (s === "?") {
        // day of week is likely setting this
        return;
      }
      if (s === "*") {
        target.cronEvery = "1";
        return;
      }
      var parts = s.split("/");
      if (parts.length === 2) {
        target.incrementStart = parts[0];
        target.incrementIncrement = parts[1];
        target.cronEvery = "3";
        return;
      }
      parts = s.split(",");
      var num = parts[0];
      if (!isNaN(num) && +num < 32) {
        target.specificSpecific = parts;
        target.cronEvery = "5";
        return;
      }
      if (s === "L") {
        target.cronEvery = "6";
        return;
      }
      if (s === "LW") {
        target.cronEvery = "7";
        return;
      }
      if (s.slice(-1) === "L") {
        target.cronLastSpecificDomDay = s.slice(0, -1);
        target.cronEvery = "8";
        return;
      }
      if (s.slice(-1) === "W") {
        target.cronDaysNearestWeekday = s.slice(0, -1);
        target.cronEvery = "10";
        return;
      }
      if (s.slice(0, 2) === "L-") {
        target.cronDaysBeforeEomMinus = s.slice(2);
        target.cronEvery = "9";
        return;
      }
      // default
      target.cronEvery = "1";
    },
    parseMonth(s) {
      var target = this.month;
      if (s === "*") {
        target.cronEvery = "1";
        return;
      }
      var parts = s.split("/");
      if (parts.length === 2) {
        target.incrementStart = parts[0];
        target.incrementIncrement = parts[1];
        target.cronEvery = "2";
        return;
      }
      parts = s.split("-");
      if (parts.length > 1) {
        target.rangeStart = parts[0];
        target.rangeEnd = parts[1];
        target.cronEvery = "4";
        return;
      }
      parts = s.split(",");
      var num = parts[0];
      if (+num < 13) {
        target.specificSpecific = parts;
        target.cronEvery = "3";
        return;
      }
      // default
      target.cronEvery = "1";
    },
    parseDow(s) {
      var target = this.week;
      if (s === "?") {
        // day of month is likely setting this
        return;
      }
      var parts = s.split("/");
      if (parts.length === 2) {
        target.incrementStart = parts[0];
        target.incrementIncrement = parts[1];
        target.cronEvery = "3";
        return;
      }
      parts = s.split("#");
      if (parts.length === 2) {
        target.cronNthDayDay = parts[0];
        target.cronNthDayNth = parts[1];
        target.cronEvery = "11";
        return;
      }
      parts = s.split(",");
      var dow = parts[0];
      if (dow.length === 3) {
        target.specificSpecific = parts;
        target.cronEvery = "5";
        return;
      }
      // default
      target.cronEvery = "?";
    },
    parseYear(s) {
      var target = this.year;
      if (s === "*") {
        target.cronEvery = "1";
        return;
      }
      var parts = s.split("/");
      if (parts.length === 2) {
        target.incrementStart = parts[0];
        target.incrementIncrement = parts[1];
        target.cronEvery = "2";
        return;
      }
      parts = s.split("-");
      if (parts.length > 1) {
        target.rangeStart = parts[0];
        target.rangeEnd = parts[1];
        target.cronEvery = "4";
        return;
      }
      parts = s.split(",");
      var num = parts[0];
      if (num.length === 4) {
        target.specificSpecific = parts;
        target.cronEvery = "3";
        return;
      }
      // default
      target.cronEvery = "1";
    }
    // rest(data) {
    //   debugger;
    //   // what is this for?
    //   for (let i in data) {
    //     if (data[i] instanceof Object) {
    //       this.rest(data[i]);
    //     } else {
    //       switch (typeof data[i]) {
    //         case "object":
    //           data[i] = [];
    //           break;
    //         case "string":
    //           data[i] = "";
    //           break;
    //       }
    //     }
    //   }
    // }
  }
};
</script>
<style lang="less">
.VueCron {
  margin: 0 auto;

  .el-select {
    width: 125px;
  }

  .tabBody {
    .el-row {
      margin: 10px 0;

      .el-input-number {
        width: 110px;
      }
    }
  }
  .result {
    text-align: center;
    .value {
      display: block;
      margin: 1em 0;
      font-size: 18px;
    }
  }
  .samples {
    .el-row {
      button {
        margin-right: 1em;
      }
    }
  }
}
</style>