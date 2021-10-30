<template>
  <div class="list-box">
    <div class="spacer-lg line" v-if="$mq === 'sm'" />
    <h1>{{ name }}</h1>
    <div class="spacer"></div>
    <div v-if="subTexts">
      <div class="spacer"></div>
      <h6 v-for="text of subTexts" :key="text">{{ text }}</h6>
      <div class="spacer"></div>
    </div>
    <div
      class="list"
      :style="{
        height: height ? height + 'vh' : '20vh',
      }"
    >
      <h3 v-if="loading">Fetching tasks...</h3>
      <br v-if="loading" />
      <ul>
        <li
          v-for="item in list"
          :key="item.name"
          @mouseover="hoverItem(item)"
          @mouseout="unHover()"
        >
          <h6 v-if="!isUrl(item)">{{ splitName(item) }}</h6>
          <a v-if="isUrl(item)" :href="url(item)" target="_blank">
            {{ splitName(item) }}
          </a>
        </li>
      </ul>
    </div>
    <div
      class="desc-box"
      :style="{
        height: height ? height / 1.5 + 'vh' : '20vh',
      }"
    >
      <p>{{ highlightedDescription }}</p>
      <p class="faded" v-if="highlightedDescription === ''">
        {{ listText }}
      </p>
    </div>
  </div>
</template>

<script>
export default {
  name: "List",
  props: {
    list: NaN,
    loading: Boolean,
    width: Number,
    height: Number,
    name: String,
    subTexts: Array,
  },
  computed: {
    listText() {
      return this.$mq === "sm"
        ? "Click on list items above to show details.."
        : "Hover over list items above to show details..";
    },
  },
  data() {
    return {
      highlightedDescription: "",
    };
  },
  methods: {
    hoverItem(item) {
      this.highlightedDescription = item.desc;
    },
    unHover() {
      this.highlightedDescription = "";
    },
    isUrl(item) {
      return item.name.includes("http");
    },
    url(item) {
      if (this.isUrl(item)) return item.name.split(" - ")[1];
    },
    splitName(item) {
      if (this.isUrl(item)) return item.name.split(" - ")[0];
      else return item.name;
    },
  },
};
</script>

<style scoped>
.list-box {
  background: rgba(255, 0, 0, 0.05);
  width: calc(100%);
  padding: 1vh;
}

.list {
  overflow-y: auto;
  overflow-x: hidden;
  width: calc(100% - 4vh);
  background: rgba(255, 0, 0, 0.0195);
  border: 1px solid rgba(255, 0, 0, 0.05);
  -webkit-box-shadow: inset 0px 0px 11px -9px rgba(255, 0, 0.05);
  box-shadow: inset 0px 0px 11px -9px rgba(255, 0, 0.05);
  padding: 1vh;
}

.desc-box {
  overflow-y: auto;
  overflow-x: hidden;
  background: white;
  margin-top: 2vw;
  margin-right: 2vw;
  width: calc(100% - 4vh);
  padding: 1vh;
  border: 1px solid rgba(255, 0, 0, 0.1);
}

.faded {
  color: rgba(0, 0, 0, 0.3);
}

.spacer {
  height: 1vh;
}

.spacer-lg {
  height: 4vh;
}

li {
  display: flex;
  flex-direction: row;
}
</style>