<template>
  <div id="tasks">
    <div class="spacer" v-if="$mq === 'sm'" />
    <h1>Experience</h1>
    <div class="spacer"></div>
    <h6>Blue Copper Coffee -- self employed</h6>
    <h6>2013 - present</h6>
    <div class="spacer"></div>
    <div id="header-box">
      <div class="spacer"></div>
      <p id="header">
        I've worn many hats running a small business for nearly a decade, some
        relevant to the tech world and some not. I ranked a few of my duties
        below from green (good at and enjoy) to red (not great at and/or don't
        enjoy). Above all, I have learned to expand my skillset, take criticism
        and most importantly, problem solve most anything that comes my way.
      </p>
    </div>
    <div class="spacer"></div>
    <div id="tasks-list">
      <h3 v-if="loading">Fetching tasks...</h3>
      <div
        v-for="task in sortedList"
        :key="task.name"
        :style="{ color: taskColor(task) }"
        class="hoverable-task"
        @mouseover="hoverItem(task)"
        @mouseout="unHover()"
      >
        <p>>>> {{ task.name }}</p>
      </div>
    </div>
    <div class="spacer"></div>
    <div class="spacer"></div>
    <div id="sort-panel">
      <div class="checkbox-panel">
        <div
          class="switch"
          :class="
            (sortValues[0] === true || loading) && !demo ? 'disabled' : ''
          "
        >
          <input
            id="checkbox-0"
            type="checkbox"
            class="switch-input"
            @change="resort(0)"
            v-model="sortValues[0]"
            :disabled="sortValues[0] === true || loading || demo"
          />
          <label for="checkbox-0" class="switch-label black"></label>
        </div>
        <p>All</p>
      </div>
      <div
        class="checkbox-panel"
        :class="(sortValues[1] === true || loading) && !demo ? 'disabled' : ''"
      >
        <div class="switch">
          <input
            id="checkbox-1"
            type="checkbox"
            class="switch-input"
            @change="resort(1)"
            v-model="sortValues[1]"
            :disabled="sortValues[1] === true || loading || demo"
          />
          <label for="checkbox-1" class="switch-label green"></label>
        </div>
        <p>Strengths</p>
      </div>
      <div
        class="checkbox-panel"
        :class="(sortValues[2] === true || loading) && !demo ? 'disabled' : ''"
      >
        <div class="switch">
          <input
            id="checkbox-2"
            type="checkbox"
            class="switch-input"
            @change="resort(2)"
            v-model="sortValues[2]"
            :disabled="sortValues[2] === true || loading || demo"
          />
          <label for="checkbox-2" class="switch-label yellow"></label>
        </div>
        <p>Average</p>
      </div>
      <div
        class="checkbox-panel"
        :class="(sortValues[3] === true || loading) && !demo ? 'disabled' : ''"
      >
        <div class="switch">
          <input
            id="checkbox-3"
            type="checkbox"
            class="switch-input"
            @change="resort(3)"
            v-model="sortValues[3]"
            :disabled="sortValues[3] === true || loading || demo"
          />
          <label for="checkbox-3" class="switch-label red"></label>
        </div>
        <p>Weaknesses</p>
      </div>
    </div>
    <div id="desc-box">
      <p>{{ highlightedDescription }}</p>
      <p class="faded" v-if="highlightedDescription === ''">
        {{ listText }}
      </p>
    </div>
  </div>
</template>

<script>
export default {
  name: "Tasks",
  props: {
    tasksList: NaN,
    loading: Boolean,
  },
  data() {
    return {
      // so we can trigger different types of sorted lists
      sortedList: [],
      sortValues: [false, false, false, false, false],
      sortType: 0,
      firstLoad: false,
      hasViewedPage: false,
      highlightedDescription: "",
      demo: false,
    };
  },
  computed: {
    listText() {
      return this.$mq === "sm"
        ? "Click on list items above to show details.."
        : "Hover over list items above to show details..";
    },
  },
  mounted() {
    window.addEventListener(
      "orientationchange",
      this.handleOrientationChange()
    );
    this.hasViewedPage = this.stringToBoolean(
      localStorage.getItem("hasViewedPage")
    );
  },
  watch: {
    tasksList(newVal) {
      if (!this.firstLoad && newVal.length > 0) {
        this.resort(0);
        this.firstLoad = true;
        if (!this.hasViewedPage)
          setTimeout(() => {
            this.demo = true;
            this.runToggles();
            localStorage.setItem("hasViewedPage", true);
          }, 1500);
      }
    },
  },
  methods: {
    stringToBoolean(string) {
      if (string) {
        switch (string.toLowerCase().trim()) {
          case "true":
          case "yes":
          case "1":
            return true;
          case "false":
          case "no":
          case "0":
          case null:
            return false;
          default:
            return Boolean(string);
        }
      }
      return false;
    },
    handleOrientationChange() {
      this.resort(this.sortType);
    },
    runToggles() {
      for (let i = 1; i < 5; i++) {
        setTimeout(() => {
          this.sortValues[i - 1] = false;
          if (i === 4) {
            i = 0;
            this.demo = false;
          }
          this.sortValues[i] = true;
          this.resort(i);
        }, i * 750);
      }
    },
    toggleDesc(task) {
      task.showDesc = !task.showDesc;
    },
    taskColor(task) {
      if (task.labels) return this.modifyColor(task.labels[0].color);
      else return "black";
    },
    hoverItem(item) {
      this.highlightedDescription = item.desc;
    },
    unHover() {
      this.highlightedDescription = "";
    },
    modifyColor(color) {
      switch (color) {
        case "red":
          return "#bb2528";
        case "orange":
          return "#f8b229";
        case "yellow":
          return "#f8b229";
        case "green":
          return "#146b3a";
        default:
          return "black";
      }
    },
    resort(sortType) {
      this.sortValues.forEach((val, index) => {
        if (index !== sortType) this.sortValues[index] = false;
        else this.sortValues[index] = true;
      });
      switch (sortType) {
        case 0:
          this.sortedList = this.tasksList.sort((a, b) => {
            const order = ["green", "yellow", "orange", "red"];
            return (
              order.indexOf(a.labels[0].color) -
              order.indexOf(b.labels[0].color)
            );
          });
          return;
        case 1:
          this.sortedList = this.tasksList.filter(
            (val) => val.labels[0].color === "green"
          );
          return;
        case 2:
          this.sortedList = this.tasksList.filter(
            (val) =>
              val.labels[0].color === "yellow" ||
              val.labels[0].color === "orange"
          );
          return;
        case 3:
          this.sortedList = this.tasksList.filter(
            (val) => val.labels[0].color === "red"
          );
          return;
      }
    },
  },
};
</script>

<style scoped>
#tasks {
  width: 100%;
  height: 93vh;
  background: rgba(255, 0, 0, 0.05);
  overflow-y: auto;
  padding: 1vh;
}

#tasks-list {
  width: calc(100% - 4vh);
  height: 40vh;
  overflow: auto;
  background: rgba(255, 0, 0, 0.0195);
  border: 1px solid rgba(255, 0, 0, 0.05);
  -webkit-box-shadow: inset 0px 0px 11px -9px rgba(255, 0, 0.05);
  box-shadow: inset 0px 0px 11px -9px rgba(255, 0, 0.05);
  padding: 1vh;
}

#desc-box {
  width: calc(100% - 4vh);
  height: 13vh;
  overflow-y: auto;
  overflow-x: hidden;
  background: white;
  margin-top: 2vw;
  margin-right: 2vw;
  padding: 1vh;
  border: 1px solid rgba(255, 0, 0, 0.1);
}

#header {
  width: calc(100% - 4vh);
}

#header-box {
  height: 11vh;
  width: calc(100% - 4vh);
  border-top: 1px solid rgba(255, 0, 0, 0.1);
  overflow-y: auto;
  overflow-x: hidden;
  padding: 1vh;
}

.checkbox-panel {
  float: left;
  width: 20%;
  height: 8vh;
  margin-bottom: 0.5vh;
  margin-right: 1vh;
  margin-top: 0.5vh;
}

.spacer {
  height: 1vh;
}

.faded {
  color: rgba(0, 0, 0, 0.3);
}

.green::after {
  background: #146b3a;
}

.red::after {
  background: #bb2528;
}

.yellow::after {
  background: #f8b229;
}

.black::after {
  background: black;
}

.hoverable-task {
  cursor: help;
}
</style>