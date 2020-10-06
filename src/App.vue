<template>
  <div>
    <K-Input-Search ref="kmember" />
    <input type="button" @click="btnClick" value="click" />
  </div>
</template>

<script>
import jsonp from "fetch-jsonp";
import moment from "moment";
import { version } from "ant-design-vue";
import debounce from "lodash/debounce";
import querystring from "querystring";
import KInputSearch from "./components/KInputSearch";

export default {
  components: { KInputSearch },
  data() {
    return {
      data: [],
      value: undefined,
    };
  },
  methods: {
    btnClick() {
      // this.value = "qianfang aa";
      var item = { key: "a001", text: "施凱文" };
      this.$refs.kmember.setValue(item);
    },
    handleSearch(value) {
      fetch(value, (data) => (this.data = data));
    },
    handleChange(value) {
      console.log(value);
      this.value = value;
      fetch(value, (data) => (this.data = data));
    },
  },
};
</script>

<style></style>
