<!--
*- Vue Application Component
-->
<template>
  <div id="container">
    <app-panel />
  </div>
</template>

<script>
/* eslint-disable */
import "bryntum-gantt/gantt.stockholm.css";
import AppPanel from "@/components/Panel.vue";

export default {
  name: "bryntum-gantt",
  components: {
    AppPanel
  } // eo components
}
</script>

<style  scoped>
@font-face {
  font-family: "Roboto";
  src: url("../../assets/fonts/Roboto-Regular.woff2") format("woff2"),
    url("../../assets/fonts/Roboto-Regular.woff") format("woff");
  font-weight: 400;
}
html,
body {
  height: 100%;
}

body {
  visibility: unset;
  font-family: "Roboto", "Helvetica Neue", Helvetica, sans-serif;
  font-size: 14px;
  margin: 0;
}

#container {
  display: flex;
  height: 100%;
  flex-direction: column;
}
#container .b-gantt {
  width: 100%;
}

#container > div.panel {
  display: flex;
  flex: 1;
}

#container > div.panel > .b-panel {
  flex: 1 1 0;
}
#container > div.panel > .b-panel > .b-toolbar {
  border-bottom: 1px solid #d8d9da;
}
#container > div.panel > .b-panel > .b-toolbar > .b-widget {
  margin-right: 1em;
}
#container > div.panel > .b-panel > .b-toolbar .b-button {
  min-width: 3.5em;
}
#container > div.panel > .b-panel .b-panel-content {
  background-color: transparent;
}
#container > div.panel > .b-panel .b-panel-content {
  padding: 0;
}

.filter-by-name label {
  display: none;
}

.b-theme-stockholm #container.b-panel > .b-toolbar {
  border-bottom-color: #d8d9da;
}

.b-theme-default #container.b-panel > .b-toolbar {
  background-color: #f1f1f4;
  border-bottom-color: #b0b0b6;
}
.b-theme-default #container.b-panel > .b-toolbar .b-has-label label {
  color: #555;
}

.b-theme-light #container.b-panel > .b-toolbar {
  background-color: #fff;
  border-bottom-color: #e0e0e0;
}

.b-theme-material #container.b-panel > .b-toolbar {
  background-color: #fff;
  border-bottom: none;
}
.b-theme-material #container.b-panel > .b-toolbar .filter-by-name label {
  display: block;
}

.b-theme-dark #container.b-panel > .b-toolbar {
  background-color: #2b2b2f;
  border-bottom-color: #2a2b2e;
}

.b-status-column-cell {
  font-size: 0.8em;
}

.b-status-column-cell i::before {
  margin-right: 0.5em;
}

.Started {
  color: #bbb;
}

.Late {
  color: #ff5722;
}

.Completed {
  color: #34d844;
}

.settings-menu .b-slider {
  margin-bottom: 0.5em;
}
</style>
