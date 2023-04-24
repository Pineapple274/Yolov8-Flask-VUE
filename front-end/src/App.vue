<template>
  <div id="app">
    <b-container>
      <b-row>
        <b-col cols="12">
          <app-header></app-header>
          <!-- 这里放置第一列 -->
        </b-col>
        <b-col cols="12">
          <app-content></app-content>
          <!-- 这里放置第二列 -->
        </b-col>
        <b-col cols="12">
          <app-footer></app-footer>
          <!-- 这里放置第三列 -->
        </b-col>
      </b-row>

    </b-container>
  </div>
</template>

<script>
import Header from "./components/Header";
import Footer from "./components/Footer";
import Content from "./components/Content";
export default {
  name: "yolo",
  data() {
    return {};
  },
  components: {
    "app-header": Header,
    "app-footer": Footer,
    "app-content": Content,
  },
  methods: {},
};
</script>

<style scope="this api replaced by slot-scope in 2.5.0+">
</style>
