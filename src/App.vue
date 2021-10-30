<template>
  <div :id="'app-' + $mq">
    <div :id="'first-' + $mq">
      <Tasks id="tasks" :tasksList="tasksList" :loading="tasksLoading" />
    </div>
    <div :id="'second-' + $mq">
      <Me id="me" />
      <List
        id="skills"
        name="skills"
        :list="skillsList"
        :loading="skillsLoading"
        :height="20"
      />
      <div class="spacer" v-if="$mq !== 'sm'"></div>
      <List
        id="projects"
        name="work examples"
        :list="projectsList"
        :loading="projectsLoading"
        :height="20"
      />
      <div class="spacer" v-if="$mq !== 'sm'"></div>
      <List
        id="hobbies"
        name="hobbies"
        :list="hobbiesList"
        :loading="hobbiesLoading"
        :height="20"
      />
    </div>
    <!-- <div id="small" v-if="$mq === 'sm'">
      <div class="spacer"></div>
      <div class="spacer"></div>
      <div class="spacer"></div>
      <div class="spacer"></div>
      <h3>Please view this site on a larger device ;)</h3> -->
    <!-- <Me id="me" />
    </div> -->
  </div>
</template>

<script>
import List from "./components/List";
import Me from "./components/Me";
import Tasks from "./components/Tasks";

import axios from "axios";

const trello_key = process.env.VUE_APP_TRELLO_KEY;
const trello_token = process.env.VUE_APP_TRELLO_TOKEN;

// TODO: move scrollbars to left side
// TODO: make the checkbox things make sense

export default {
  name: "App",
  components: {
    List,
    Me,
    Tasks,
  },
  data() {
    return {
      skillsList: [],
      tasksList: [],
      projectsList: [],
      hobbiesList: [],
      skillsLoading: true,
      tasksLoading: true,
      projectsLoading: true,
      hobbiesLoading: true,
    };
  },
  methods: {
    loadTrelloCards(list) {
      return axios.get(
        `https://api.trello.com/1/lists/${list}/cards?fields=name,labels,desc&key=${trello_key}&token=${trello_token}`
      );
    },
  },
  created() {
    // task list
    this.loadTrelloCards("5fdd186f43fd7a8cdedc9990").then((res) => {
      this.tasksList = res.data;
      this.tasksLoading = false;
    });
    // skill list
    this.loadTrelloCards("5fdf841744c85f8cb1449212").then((res) => {
      this.skillsList = res.data;
      this.skillsLoading = false;
    });
    // projects list
    this.loadTrelloCards("5fe042d8b7521d1e4dc1aefe").then((res) => {
      this.projectsList = res.data;
      this.projectsLoading = false;
    });
    // hobbies list
    this.loadTrelloCards("5fdf841481d14d0a06ce07c5").then((res) => {
      this.hobbiesList = res.data;
      this.hobbiesLoading = false;
    });
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=DM+Mono:ital,wght@0,300;0,500;1,400&display=swap");

@font-face {
  font-family: "Manufaktur Ultra Expanded Ex Bd";
  src: url("assets/fonts/Manufaktur-UltraExpandedExtraBold.woff") format("woff");
  font-weight: 800;
  font-style: normal;
  font-display: swap;
}

@font-face {
  font-family: "Manufaktur Ultra Expanded";
  src: url("assets/fonts/Manufaktur-UltraExpandedBold.woff") format("woff");
  font-weight: bold;
  font-style: normal;
  font-display: swap;
}

@font-face {
  font-family: "Manufaktur Expanded";
  src: url("assets/fonts/Manufaktur-ExpandedBold.woff") format("woff");
  font-weight: bold;
  font-style: normal;
  font-display: swap;
}

#app-lg {
  height: 98vh;
  width: 98vw;
  overflow: hidden;
  display: block;
  padding: 2vh;
}

#app-sm {
  height: 100vh;
  width: 100vw;
  overflow-x: hidden;
  overflow-y: scroll;
  display: block;
}

body {
  overflow: hidden;
}

#second-lg {
  float: right;
  width: 34vw;
  height: 95vh;
  margin: 0.5vw;
  overflow-y: auto;
  overflow-x: hidden;
}

#first-lg {
  float: left;
  width: 60vw;
  height: 95vh;
  margin: 0.5vw;
  overflow-y: auto;
  overflow-x: hidden;
}

#first-sm {
  width: 98vw;
  height: 95vh;
   margin-left: 0.5vw;
  margin-right: 0.5vw;
  overflow-y: auto;
  overflow-x: hidden;
}

#second-sm {
  width: 98vw;
  max-height: 2000vh;
  margin-left: 0.5vw;
  margin-right: 0.5vw;
  overflow-y: auto;
  overflow-x: hidden;
}

#small {
  text-align: center;
}

.shadow-left-down {
  -webkit-box-shadow: -2px 2px 7px 0px rgba(0, 0, 0, 0.75);
  -moz-box-shadow: -2px 2px 7px 0px rgba(0, 0, 0, 0.75);
  box-shadow: -2px 2px 7px 0px rgba(0, 0, 0, 0.75);
}

.spacer {
  height: 5vh;
}

.line {
  width: 70%;
  border-top: .75px solid rgba(255, 0, 0, .2);
}

.switch {
  position: relative;
  display: inline-block;
}

.switch-input {
  display: none;
}

.switch-label {
  display: block;
  width: 6vh;
  height: 3vh;
  text-indent: -150%;
  clip: rect(0 0 0 0);
  color: transparent;
  user-select: none;
}

.switch-label::before,
.switch-label::after {
  content: "";
  display: block;
  position: absolute;
  cursor: pointer;
}

.switch-label::before {
  width: 100%;
  height: 100%;
  background-color: rgba(255, 0, 0, 0.2);
  -webkit-transition: background-color 0.25s ease;
  transition: background-color 0.25s ease;
}

.switch-label::after {
  top: 0;
  left: 0;
  width: 3vh;
  height: 3vh;
  background-color: #fff;
  box-shadow: 0 0 2px rgba(0, 0, 0, 0.45);
  -webkit-transition: left 0.25s ease;
  transition: left 0.25s ease;
}

.switch-input:checked + .switch-label::before {
  background-color: rgba(255, 0, 0, 0.8);
  cursor: not-allowed;
}

.switch-input:checked + .switch-label::after {
  left: 3vh;
  border: 1px solid rgba(255, 0, 0, 0.3);
  cursor: not-allowed;
}

.disabled {
  /* IE 8 */
  -ms-filter: "progid:DXImageTransform.Microsoft.Alpha(Opacity=50)";

  /* IE 5-7 */
  filter: alpha(opacity=50);

  /* Netscape */
  -moz-opacity: 0.5;

  /* Safari 1.x */
  -khtml-opacity: 0.5;

  /* Good browsers */
  opacity: 0.5;
}

h1 {
  font-family: "Manufaktur Ultra Expanded Ex Bd";
  /* background: rgba(255, 0, 0, 0.1); */
}

h2 {
  font-family: "Manufaktur Ultra Expanded Ex Bd";
  /* background: rgba(255, 0, 0, 0.1); */
}

h3 {
  font-family: "Manufaktur Ultra Expanded Ex Bd";
}

h4 {
  font-family: "Manufaktur Expanded";
}

h5 {
  font-family: "Manufaktur Expanded";
}

h6 {
  font-family: "Manufaktur Expanded";
}

body p {
  font-family: "DM Mono";
}

li {
  cursor: help;
}

a {
  font-family: "DM Mono";
  color: rgba(255, 0, 0, 0.5);
}

/* BEGIN CSS RESET */

html,
body,
div,
span,
applet,
object,
iframe,
h1,
h2,
h3,
h4,
h5,
h6,
p,
blockquote,
pre,
a,
abbr,
acronym,
address,
big,
cite,
code,
del,
dfn,
em,
img,
ins,
kbd,
q,
s,
samp,
small,
strike,
strong,
sub,
sup,
tt,
var,
b,
u,
i,
center,
dl,
dt,
dd,
ol,
ul,
li,
fieldset,
form,
label,
legend,
table,
caption,
tbody,
tfoot,
thead,
tr,
th,
td,
article,
aside,
canvas,
details,
embed,
figure,
figcaption,
footer,
header,
hgroup,
menu,
nav,
output,
ruby,
section,
summary,
time,
mark,
audio,
video {
  margin: 0;
  padding: 0;
  border: 0;
  font-size: 100%;
  vertical-align: baseline;
}
/* HTML5 display-role reset for older browsers */
article,
aside,
details,
figcaption,
figure,
footer,
header,
hgroup,
menu,
nav,
section {
  display: block;
}
body {
  line-height: 1;
}
ol,
ul {
  list-style: none;
}
blockquote,
q {
  quotes: none;
}
blockquote:before,
blockquote:after,
q:before,
q:after {
  content: "";
  content: none;
}
table {
  border-collapse: collapse;
  border-spacing: 0;
}

/* END CSS RESET */

::-webkit-scrollbar {
  width: 0.75vh;
  height: 0.75vh;
  cursor: auto;
}
::-webkit-scrollbar-button {
  width: 0px;
  height: 0px;
}
::-webkit-scrollbar-thumb {
  background: rgba(255, 0, 0, 0.2);
  /* border: 0.25vh solid #ffffff; */
  /* border-radius: 50px; */
}
::-webkit-scrollbar-thumb:hover {
  background: rgba(255, 0, 0, 0.35);
}
::-webkit-scrollbar-thumb:active {
  background: rgba(255, 0, 0, 0.35);
}
::-webkit-scrollbar-track {
  background: #ffffff;
  border: 0px none #ffffff;
  /* border-radius: 50px; */
}
::-webkit-scrollbar-track:hover {
  background: #ffffff;
}
::-webkit-scrollbar-track:active {
  background: rgba(255, 0, 0, 0.1);
}
::-webkit-scrollbar-corner {
  background: transparent;
}
</style>
