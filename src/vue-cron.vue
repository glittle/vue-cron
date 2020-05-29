<template>
  <div class="VueCron">
    <div :class="{forDebug: debug}">
      <div>
        <el-tabs type="border-card" :value="tabNum">
          <el-tab-pane>
            <span slot="label">
              <i class="el-icon-info"></i>
              {{text.Samples.name}}
            </span>
            <div class="tabBody samples">
              <el-row>
                <el-button size="small" v-on:click="use(1)">Use</el-button>
                <span ref="s1" data-cron="0 0 0 ? * TUE *">Every Tuesday</span>
              </el-row>
              <el-row>
                <el-button size="small" v-on:click="use(2)">Use</el-button>
                <span ref="s2" data-cron="0 0 0 21 * ? *">Every month on the 21st</span>
              </el-row>
              <el-row>
                <el-button size="small" v-on:click="use(3)">Use</el-button>
                <span ref="s3" data-cron="0 0 0 1 1/3 ? *">First day of every quarter</span>
              </el-row>
              <el-row>
                <el-button size="small" v-on:click="use(4)">Use</el-button>
                <span ref="s4" data-cron="0 0 0 1W 10 ? *">The first weekday in October</span>
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
                <span v-if="debug" class="debugMode">1</span>
                <el-radio v-model="month.mode" label="1">
                  {{text.Month.every}}
                  <span></span>
                </el-radio>
              </el-row>
              <el-row>
                <span v-if="debug" class="debugMode">3</span>
                <el-radio class="long" v-model="month.mode" label="3">
                  {{text.Month.specific}}
                  <el-select size="small" multiple v-model="month.specific">
                    <el-option
                      v-for="(val,i) in text.Month.months"
                      :key="val"
                      :label="val"
                      :value="''+(i+1)"
                    ></el-option>
                  </el-select>
                </el-radio>
              </el-row>
              <el-row>
                <span v-if="debug" class="debugMode">2</span>
                <el-radio v-model="month.mode" label="2">
                  {{text.Month.interval[0]}}
                  <el-select size="small" v-model="month.incrementInc">
                    <el-option v-for="(val,i) in ordinals12" :key="val" :label="val" :value="i"></el-option>
                  </el-select>
                  {{text.Month.interval[1]}}
                  <el-select size="small" v-model="month.incrementStart">
                    <el-option
                      v-for="(val,i) in text.Month.months"
                      :key="val"
                      :label="val"
                      :value="(i+1)"
                    ></el-option>
                  </el-select>
                </el-radio>
              </el-row>

              <el-row>
                <span v-if="debug" class="debugMode">4</span>
                <el-radio v-model="month.mode" label="4">
                  {{text.Month.cycle[0]}}
                  <el-select size="small" v-model="month.rangeStart">
                    <el-option
                      v-for="(val,i) in text.Month.months"
                      :key="val"
                      :label="val"
                      :value="''+(i+1)"
                    ></el-option>
                  </el-select>
                  {{text.Month.cycle[1]}}
                  <el-select size="small" v-model="month.rangeEnd">
                    <el-option
                      v-for="(val,i) in text.Month.months"
                      :key="val"
                      :label="val"
                      :value="''+(i+1)"
                    ></el-option>
                  </el-select>
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
                <span v-if="debug" class="debugMode">1</span>
                <el-radio v-model="day.mode" label="1">
                  {{text.Day.every}}
                  <span></span>
                </el-radio>
              </el-row>
              <el-row>
                <span v-if="debug" class="debugMode">5</span>
                <el-radio class="long" v-model="day.mode" label="5">
                  {{text.Day.specificDay}}
                  <el-select size="small" multiple v-model="day.specific">
                    <el-option v-for="val in 31" :key="val" :value="val">{{val}}</el-option>
                  </el-select>
                </el-radio>
              </el-row>
              <el-row>
                <span v-if="debug" class="debugMode">4</span>
                <el-radio class="long" v-model="day.mode" label="4">
                  {{text.Day.specificWeek}}
                  <el-select size="small" multiple v-model="week.specific">
                    <el-option
                      v-for="val in 7"
                      :key="val"
                      :label="text.Week.weeks[val-1]"
                      :value="['SUN','MON','TUE','WED','THU','FRI','SAT'][val-1]"
                    ></el-option>
                  </el-select>
                </el-radio>
              </el-row>
              <el-row>
                <span v-if="debug" class="debugMode">3</span>
                <el-radio v-model="day.mode" label="3">
                  {{text.Day.intervalDay[0]}}
                  <el-input-number size="small" v-model="day.incrementInc" :min="1" :max="31"></el-input-number>
                  {{text.Day.intervalDay[1]}}
                  <el-input-number size="small" v-model="day.incrementStart" :min="1" :max="31"></el-input-number>
                  {{text.Day.intervalDay[2]}}
                </el-radio>
              </el-row>
              <el-row>
                <span v-if="debug" class="debugMode">2</span>
                <el-radio v-model="day.mode" label="2">
                  {{text.Day.intervalWeek[0]}}
                  <el-input-number size="small" v-model="week.incrementInc" :min="1" :max="7"></el-input-number>
                  {{text.Day.intervalWeek[1]}}
                  <el-select size="small" v-model="week.incrementStart">
                    <el-option
                      v-for="val in 7"
                      :key="val"
                      :label="text.Week.weeks[val-1]"
                      :value="val"
                    ></el-option>
                  </el-select>
                  {{text.Day.intervalWeek[2]}}
                </el-radio>
              </el-row>
              <el-row>
                <span v-if="debug" class="debugMode">10</span>
                <el-radio v-model="day.mode" label="10">
                  {{text.Day.nearestWeekday[0]}}
                  <el-input-number size="small" v-model="day.nearestWeekday" :min="1" :max="31"></el-input-number>
                  {{text.Day.nearestWeekday[1]}}
                </el-radio>
              </el-row>
              <el-row>
                <span v-if="debug" class="debugMode">11</span>
                <el-radio v-model="day.mode" label="11">
                  {{text.Day.someWeekday[0]}}
                  <el-select size="small" v-model="week.nthOrdinal">
                    <el-option v-for="(val,i) in ordinals5" :key="val" :label="val" :value="''+(i+1)"></el-option>
                  </el-select>&nbsp;
                  <el-select size="small" v-model="week.nthDow">
                    <el-option
                      v-for="val in 7"
                      :key="val"
                      :label="text.Week.weeks[val-1]"
                      :value="val"
                    ></el-option>
                  </el-select>
                  {{text.Day.someWeekday[1]}}
                </el-radio>
              </el-row>
              <el-row>
                <span v-if="debug" class="debugMode">8</span>
                <el-radio v-model="day.mode" label="8">
                  {{text.Day.lastWeek[0]}}
                  <el-select size="small" v-model="day.lastSpecificDom">
                    <el-option
                      v-for="val in 7"
                      :key="val"
                      :label="text.Week.weeks[val-1]"
                      :value="val"
                    ></el-option>
                  </el-select>
                  {{text.Day.lastWeek[1]||''}}
                </el-radio>
              </el-row>
              <el-row>
                <span v-if="debug" class="debugMode">7</span>
                <el-radio v-model="day.mode" label="7">
                  {{text.Day.lastWeekday}}
                  <span></span>
                </el-radio>
              </el-row>
              <el-row>
                <span v-if="debug" class="debugMode">9</span>
                <el-radio v-model="day.mode" label="9">
                  {{text.Day.beforeEndMonth[0]}}
                  <el-input-number size="small" v-model="day.daysBeforeEom" :min="1" :max="31"></el-input-number>
                  {{text.Day.beforeEndMonth[1]}}
                </el-radio>
              </el-row>

              <el-row>
                <span v-if="debug" class="debugMode">6</span>
                <el-radio v-model="day.mode" label="6">
                  {{text.Day.lastDay}}
                  <span></span>
                </el-radio>
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
                <span v-if="debug" class="debugMode">1</span>
                <el-radio v-model="year.mode" label="1">
                  {{text.Year.every}}
                  <span></span>
                </el-radio>
              </el-row>
              <el-row>
                <span v-if="debug" class="debugMode">2</span>
                <el-radio v-model="year.mode" label="2">
                  {{text.Year.interval[0]}}
                  <el-input-number size="small" v-model="year.incrementInc" :min="1" :max="99"></el-input-number>
                  {{text.Year.interval[1]}}
                  <el-input-number
                    size="small"
                    v-model="year.incrementStart"
                    :min="2000"
                    :max="2200"
                  ></el-input-number>
                </el-radio>
              </el-row>
              <el-row>
                <span v-if="debug" class="debugMode">3</span>
                <el-radio class="long" v-model="year.mode" label="3">
                  {{text.Year.specific}}
                  <el-select size="small" filterable multiple v-model="year.specific">
                    <el-option v-for="val in 100" :key="val" :label="2019+val" :value="2019+val"></el-option>
                  </el-select>
                </el-radio>
              </el-row>
              <el-row>
                <span v-if="debug" class="debugMode">4</span>
                <el-radio v-model="year.mode" label="4">
                  {{text.Year.cycle[0]}}
                  <el-input-number size="small" v-model="year.rangeStart" :min="2000" :max="2200"></el-input-number>
                  {{text.Year.cycle[1]}}
                  <el-input-number size="small" v-model="year.rangeEnd" :min="2000" :max="2200"></el-input-number>
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
                <span v-if="debug" class="debugMode">5</span>
                <el-radio v-model="hour.mode" label="5">
                  {{text.Hours.midnight}}
                  <span></span>
                </el-radio>
              </el-row>
              <el-row>
                <span v-if="debug" class="debugMode">1</span>
                <el-radio v-model="hour.mode" label="1">
                  {{text.Hours.every}}
                  <span></span>
                </el-radio>
              </el-row>
              <el-row>
                <span v-if="debug" class="debugMode">3</span>
                <el-radio class="long" v-model="hour.mode" label="3">
                  {{text.Hours.specific}}
                  <el-select size="small" multiple v-model="hour.specific">
                    <el-option v-for="val in 24" :key="val" :value="val-1">{{val-1}}</el-option>
                  </el-select>
                </el-radio>
              </el-row>
              <el-row>
                <span v-if="debug" class="debugMode">2</span>
                <el-radio v-model="hour.mode" label="2">
                  {{text.Hours.interval[0]}}
                  <el-input-number size="small" v-model="hour.incrementInc" :min="0" :max="23"></el-input-number>
                  {{text.Hours.interval[1]}}
                  <el-input-number size="small" v-model="hour.incrementStart" :min="0" :max="23"></el-input-number>
                  {{text.Hours.interval[2]}}
                </el-radio>
              </el-row>

              <el-row>
                <span v-if="debug" class="debugMode">4</span>
                <el-radio v-model="hour.mode" label="4">
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
                <span v-if="debug" class="debugMode">5</span>
                <el-radio v-model="minute.mode" label="5">
                  {{text.Minutes.top}}
                  <span></span>
                </el-radio>
              </el-row>
              <el-row>
                <span v-if="debug" class="debugMode">1</span>
                <el-radio v-model="minute.mode" label="1">
                  {{text.Minutes.every}}
                  <span></span>
                </el-radio>
              </el-row>
              <el-row>
                <span v-if="debug" class="debugMode">2</span>
                <el-radio v-model="minute.mode" label="2">
                  {{text.Minutes.interval[0]}}
                  <el-input-number size="small" v-model="minute.incrementInc" :min="1" :max="60"></el-input-number>
                  {{text.Minutes.interval[1]}}
                  <el-input-number size="small" v-model="minute.incrementStart" :min="0" :max="59"></el-input-number>
                  {{text.Minutes.interval[2]||''}}
                </el-radio>
              </el-row>
              <el-row>
                <span v-if="debug" class="debugMode">3</span>
                <el-radio class="long" v-model="minute.mode" label="3">
                  {{text.Minutes.specific}}
                  <el-select size="small" multiple v-model="minute.specific">
                    <el-option v-for="val in 60" :key="val" :value="val-1">{{val-1}}</el-option>
                  </el-select>
                </el-radio>
              </el-row>
              <el-row>
                <span v-if="debug" class="debugMode">4</span>
                <el-radio v-model="minute.mode" label="4">
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
                <span v-if="debug" class="debugMode">5</span>
                <el-radio v-model="second.mode" label="5">
                  {{text.Seconds.top}}
                  <span></span>
                </el-radio>
              </el-row>
              <el-row>
                <span v-if="debug" class="debugMode">1</span>
                <el-radio v-model="second.mode" label="1">
                  {{text.Seconds.every}}
                  <span></span>
                </el-radio>
              </el-row>
              <el-row>
                <span v-if="debug" class="debugMode">2</span>
                <el-radio v-model="second.mode" label="2">
                  {{text.Seconds.interval[0]}}
                  <el-input-number size="small" v-model="second.incrementInc" :min="1" :max="60"></el-input-number>
                  {{text.Seconds.interval[1]||''}}
                  <el-input-number size="small" v-model="second.incrementStart" :min="0" :max="59"></el-input-number>
                  {{text.Seconds.interval[2]||''}}
                </el-radio>
              </el-row>
              <el-row>
                <span v-if="debug" class="debugMode">3</span>
                <el-radio class="long" v-model="second.mode" label="3">
                  {{text.Seconds.specific}}
                  <el-select size="small" multiple v-model="second.specific">
                    <el-option v-for="val in 60" :key="val" :value="val-1">{{val-1}}</el-option>
                  </el-select>
                </el-radio>
              </el-row>
              <el-row>
                <span v-if="debug" class="debugMode">4</span>
                <el-radio v-model="second.mode" label="4">
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
          <span class="value" v-html="this.finalHtml"></span>
          <el-button size="small" type="primary" @click="change">{{text.Save}}</el-button>
          <el-button size="small" @click="close">{{text.Cancel}}</el-button>
        </div>
      </div>
      <div class="rawDebug" v-if="debug" v-html="JSON.stringify($data, undefined, 2)"></div>
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
      default: true
    },
    showHours: {
      type: Boolean,
      default: true
    },
    lang: String,
    debug: Boolean
  },
  data() {
    return {
      second: {
        mode: "",
        incrementStart: 3,
        incrementInc: 5,
        rangeStart: "",
        rangeEnd: "",
        specific: ["0"]
      },
      minute: {
        mode: "",
        incrementStart: 3,
        incrementInc: 5,
        rangeStart: "",
        rangeEnd: "",
        specific: ["0"]
      },
      hour: {
        mode: "",
        incrementStart: 3,
        incrementInc: 5,
        rangeStart: "",
        rangeEnd: "",
        specific: ["0"]
      },
      day: {
        mode: "",
        incrementStart: 1,
        incrementInc: 1,
        rangeStart: "",
        rangeEnd: "",
        specific: ["1"],
        lastSpecificDom: 1,
        daysBeforeEom: "",
        nearestWeekday: ""
      },
      week: {
        // mode: "",
        incrementStart: 1,
        incrementInc: 1,
        specific: ["SUN"],
        nthDow: 1,
        nthOrdinal: 1
      },
      month: {
        mode: "",
        incrementStart: 3,
        incrementInc: 5,
        rangeStart: "",
        rangeEnd: "",
        specific: ["1"]
      },
      year: {
        mode: "",
        incrementStart: 2020,
        incrementInc: 1,
        rangeStart: "",
        rangeEnd: "",
        specific: []
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
    tabNum() {
      return "1";
    },
    ordinals12() {
      return this.text.Ordinal.slice(0, 13);
    },
    ordinals5() {
      return this.text.Ordinal.slice(1, 6);
    },
    ordinalDays() {
      return this.text.Ordinal.slice(1, 31);
    },
    secondsText() {
      let seconds = "";
      let mode = this.second.mode;
      switch (mode.toString()) {
        case "1":
          seconds = "*";
          break;
        case "2":
          seconds = this.second.incrementStart + "/" + this.second.incrementInc;
          break;
        case "3":
          seconds = this.second.specific.join(",");
          break;
        case "4":
          seconds = this.second.rangeStart + "-" + this.second.rangeEnd;
          break;
        case "5":
          seconds = "0";
          break;
      }
      return seconds;
    },
    minutesText() {
      let minutes = "";
      let mode = this.minute.mode;
      switch (mode.toString()) {
        case "1":
          minutes = "*";
          break;
        case "2":
          minutes = this.minute.incrementStart + "/" + this.minute.incrementInc;
          break;
        case "3":
          minutes = this.minute.specific.join(",");
          break;
        case "4":
          minutes = this.minute.rangeStart + "-" + this.minute.rangeEnd;
          break;
        case "5":
          minutes = "0";
          break;
      }
      return minutes;
    },
    hoursText() {
      let hours = "";
      let mode = this.hour.mode;
      switch (mode.toString()) {
        case "1":
          hours = "*";
          break;
        case "2":
          hours = this.hour.incrementStart + "/" + this.hour.incrementInc;
          break;
        case "3":
          hours = this.hour.specific.join(",");
          break;
        case "4":
          hours = this.hour.rangeStart + "-" + this.hour.rangeEnd;
          break;
        case "5": // midnight
          hours = "0";
          break;
      }
      return hours;
    },
    daysText() {
      let days = "";
      let mode = this.day.mode;
      switch (mode.toString()) {
        case "1":
          days = "*";
          break;
        case "2":
        case "4":
        case "11":
          days = "?";
          break;
        case "3":
          days = this.day.incrementStart + "/" + this.day.incrementInc;
          break;
        case "5":
          days = this.day.specific.join(",");
          break;
        case "6":
          days = "L";
          break;
        case "7":
          days = "LW";
          break;
        case "8":
          days = this.day.lastSpecificDom + "L";
          break;
        case "9":
          days = "L-" + this.day.daysBeforeEom;
          break;
        case "10":
          days = this.day.nearestWeekday + "W";
          break;
      }
      return days;
    },
    weeksText() {
      let weeks = "";
      let mode = this.day.mode; // based on DAY mode
      switch (mode.toString()) {
        case "1":
        case "3":
        case "5":
          weeks = "?";
          break;
        case "2":
          weeks = this.week.incrementStart + "/" + this.week.incrementInc;
          break;
        case "4":
          weeks = this.week.specific.join(",");
          break;
        case "6":
        case "7":
        case "8":
        case "9":
        case "10":
          weeks = "?";
          break;
        case "11":
          weeks = this.week.nthDow + "#" + this.week.nthOrdinal;
          break;
      }
      return weeks;
    },
    monthsText() {
      let months = "";
      let mode = this.month.mode;
      switch (mode.toString()) {
        case "1":
          months = "*";
          break;
        case "2":
          months = this.month.incrementStart + "/" + this.month.incrementInc;
          break;
        case "3":
          months = this.month.specific.join(",");
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
      let mode = target.mode;
      switch (mode.toString()) {
        case "1":
          years = "*";
          break;
        case "2":
          years = target.incrementStart + "/" + target.incrementInc;
          break;
        case "3":
          years = target.specific.join(",");
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
    },
    finalHtml() {
      return `<span class="Seconds" title="${this.text.Seconds.name}">${this
        .secondsText || "*"}</span>
       <span class="Minutes" title="${this.text.Minutes.name}">${this
        .minutesText || "*"}</span>
       <span class="Hours" title="${this.text.Hours.name}">${this.hoursText ||
        "*"}</span>
       <span class="Days" title="${this.text.Day.name}">${this.daysText ||
        "*"}</span>
       <span class="Months" title="${this.text.Month.name}">${this.monthsText ||
        "*"}</span>
       <span class="Weeks" title="${this.text.Week.name}">${this.weeksText ||
        "?"}</span>
       <span class="Years" title="${this.text.Year.name}">${this.yearsText ||
        "*"}`;
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
        this.$emit("input", this.final);
      }
    },
    parse(s) {
      this.reset(this.$data);
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
      this.parseDay(parts[3], parts[5]);
      this.parseMonth(parts[4]);
      this.parseYear(parts[6]);
    },
    parseSecond(s) {
      var target = this.second;
      if (s === "*") {
        target.mode = "1";
        return;
      }
      if (s === "0") {
        target.specific = [s];
        target.mode = "5";
        return;
      }
      var parts = s.split("/");
      if (parts.length === 2) {
        target.incrementStart = +parts[0];
        target.incrementInc = +parts[1];
        target.mode = "2";
        return;
      }
      parts = s.split("-");
      if (parts.length > 1) {
        target.rangeStart = +parts[0];
        target.rangeEnd = +parts[1];
        target.mode = "4";
        return;
      }
      parts = s.split(",");
      var num = parts[0];
      if (+num < 60) {
        target.specific = parts;
        target.mode = "3";
        return;
      }
      // default
      target.mode = "1";
    },
    parseMinute(s) {
      var target = this.minute;
      if (s === "*") {
        target.mode = "1";
        return;
      }
      if (s === "0") {
        target.specific = [s];
        target.mode = "5";
        return;
      }
      var parts = s.split("/");
      if (parts.length === 2) {
        target.incrementStart = +parts[0];
        target.incrementInc = +parts[1];
        target.mode = "2";
        return;
      }
      parts = s.split("-");
      if (parts.length > 1) {
        target.rangeStart = +parts[0];
        target.rangeEnd = +parts[1];
        target.mode = "4";
        return;
      }
      parts = s.split(",");
      var num = parts[0];
      if (+num < 60) {
        target.specific = parts;
        target.mode = "3";
        return;
      }
      // default
      target.mode = "1";
    },
    parseHour(s) {
      var target = this.hour;
      if (s === "*") {
        target.mode = "1";
        return;
      }
      if (s === "0") {
        target.specific = [s];
        target.mode = "5";
        return;
      }
      var parts = s.split("/");
      if (parts.length === 2) {
        target.incrementStart = +parts[0];
        target.incrementInc = +parts[1];
        target.mode = "2";
        return;
      }
      parts = s.split("-");
      if (parts.length > 1) {
        target.rangeStart = +parts[0];
        target.rangeEnd = +parts[1];
        target.mode = "4";
        return;
      }
      parts = s.split(",");
      var num = parts[0];
      if (+num < 24) {
        target.specific = parts;
        target.mode = "3";
        return;
      }
      // default
      target.mode = "1";
    },
    parseDay(dom, dow) {
      var targetDom = this.day;
      var targetDow = this.week;
      var parts;
      if (dom === "*") {
        targetDom.mode = "1";
        return;
      }
      if (dom === "?") {
        parts = dow.split("/");
        if (parts.length === 2) {
          targetDow.incrementStart = +parts[0];
          targetDow.incrementInc = +parts[1];
          targetDom.mode = "2";
          return;
        }
        parts = dow.split("#");
        if (parts.length === 2) {
          targetDow.nthDow = +parts[0];
          targetDow.nthOrdinal = +parts[1];
          targetDom.mode = "11";
          return;
        }
        parts = dow.split(",");
        var dayCode = parts[0];
        if (dayCode.length === 3) {
          targetDow.specific = parts;
          targetDom.mode = "4";
          return;
        }
      }
      parts = dom.split("/");
      if (parts.length === 2) {
        targetDom.incrementStart = +parts[0];
        targetDom.incrementInc = +parts[1];
        targetDom.mode = "3";
        return;
      }
      parts = dom.split(",");
      var num = parts[0];
      if (!isNaN(num) && +num < 32) {
        targetDom.specific = parts;
        targetDom.mode = "5";
        return;
      }
      if (dom === "L") {
        targetDom.mode = "6";
        return;
      }
      if (dom === "LW") {
        targetDom.mode = "7";
        return;
      }
      if (dom.slice(-1) === "L") {
        targetDom.lastSpecificDom = +dom.slice(0, -1);
        targetDom.mode = "8";
        return;
      }
      if (dom.slice(-1) === "W") {
        targetDom.nearestWeekday = +dom.slice(0, -1);
        targetDom.mode = "10";
        return;
      }
      if (dom.slice(0, 2) === "L-") {
        targetDom.daysBeforeEom = +dom.slice(2);
        targetDom.mode = "9";
        return;
      }
      // default
      targetDom.mode = "1";
    },
    // parseDow(s) {
    //   var target = this.week;
    //   if (dow === "?") {
    //     // day of month is likely setting this
    //     return;
    //   }
    //   var parts = dow.split("/");
    //   if (parts.length === 2) {
    //     targetDow.incrementStart = +parts[0];
    //     targetDow.incrementInc = +parts[1];
    //     targetDow.mode = "3";
    //     return;
    //   }
    //   parts = dow.split("#");
    //   if (parts.length === 2) {
    //     targetDow.nthDow = +parts[0];
    //     targetDow.nthOrdinal = +parts[1];
    //     targetDow.mode = "11";
    //     return;
    //   }
    //   parts = dow.split(",");
    //   var dow = parts[0];
    //   if (dow.length === 3) {
    //     targetDow.specific = parts;
    //     targetDow.mode = "4";
    //     return;
    //   }
    //   // default
    //   target.mode = "?";
    // },
    parseMonth(s) {
      var target = this.month;
      if (s === "*") {
        target.mode = "1";
        return;
      }
      var parts = s.split("/");
      if (parts.length === 2) {
        target.incrementStart = +parts[0];
        target.incrementInc = +parts[1];
        target.mode = "2";
        return;
      }
      parts = s.split("-");
      if (parts.length > 1) {
        target.rangeStart = +parts[0];
        target.rangeEnd = +parts[1];
        target.mode = "4";
        return;
      }
      parts = s.split(",");
      var num = parts[0];
      if (+num < 13) {
        target.specific = parts;
        target.mode = "3";
        return;
      }
      // default
      target.mode = "1";
    },

    parseYear(s) {
      var target = this.year;
      if (s === "*") {
        target.mode = "1";
        return;
      }
      var parts = s.split("/");
      if (parts.length === 2) {
        target.incrementStart = +parts[0];
        target.incrementInc = +parts[1];
        target.mode = "2";
        return;
      }
      parts = s.split("-");
      if (parts.length > 1) {
        target.rangeStart = +parts[0];
        target.rangeEnd = +parts[1];
        target.mode = "4";
        return;
      }
      parts = s.split(",");
      var num = parts[0];
      if (num.length === 4) {
        target.specific = parts;
        target.mode = "3";
        return;
      }
      // default
      target.mode = "1";
    },
    reset(data) {
      for (let i in data) {
        if (data[i] instanceof Object) {
          this.reset(data[i]);
        } else {
          switch (typeof data[i]) {
            case "object":
              data[i] = [];
              break;
            case "string":
              data[i] = "";
              break;
          }
        }
      }
    }
  }
};
</script>
<style lang="less">
.VueCron {
  margin: 0 auto;
  counter-reset: row;

  .el-select {
    width: 125px;
  }

  .tabBody {
    .el-row {
      margin: 10px 0;
      position: relative;

      .el-input-number {
        width: 110px;
      }
    }
    // .el-row::before {
    //   counter-increment: row;
    //   content: counter(row);
    //   position: absolute;
    //   left: -14px;
    //   top: 2px;
    //   color: blue;
    //   font-size: 70%;
    // }
  }
  .result {
    text-align: center;
    .value {
      display: block;
      margin: 1em 0;
      font-size: 18px;
    }
    .Seconds,
    .Minutes,
    .Hours {
      color: blue;
    }
    .Days {
      color: darkgreen;
    }
    .Months {
      color: purple;
    }
    .Weeks {
      color: magenta;
    }
    .Years {
      color: navy;
    }
  }
  .samples {
    .el-row {
      button {
        margin-right: 1em;
      }
    }
  }
  .forDebug {
    display: flex;
  }
  .rawDebug {
    white-space: pre;
    max-height: 75vh;
    overflow: auto;
  }
  span.debugMode {
    color: red;
    font-size: 80%;
    display: inline-block;
    width: 15px;
    text-align: right;
    padding-right: 5px;
  }
}
</style>