 <template>
  <div>
    <a-select
      show-search
      :value="value"
      placeholder="input search text"
      style="width: 200px"
      :default-active-first-option="false"
      :show-arrow="false"
      :filter-option="false"
      :not-found-content="null"
      @search="handleSearch"
      @change="handleChange"
    >
      <a-select-option v-for="d in data" :key="d.key">
        {{ d.text }}
      </a-select-option>
    </a-select>
  </div>
</template>

<script>
import jsonp from "fetch-jsonp";
import moment from "moment";
import { version } from "ant-design-vue";
import debounce from "lodash/debounce";
import querystring from "querystring";
let timeout;
let currentValue;

export default {
  name: "KInputSearch",
  data() {
    return {
      data: [],
      value: undefined,
      key: undefined,
    };
  },
  methods: {
    // 直接プルダウンのその値を設置します
    setValue(row) {
      this.data = [row];
      this.key = row.key;
      this.value = row.text;
    },
    btnClick() {
      alert("ok");
      this.value = "qianfang aa";
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

function fetch(value, callback) {
  if (timeout) {
    clearTimeout(timeout);
    timeout = null;
  }
  currentValue = value;

  function fake() {
    const str = querystring.encode({
      code: "utf-8",
      q: value,
    });
    jsonp(`https://suggest.taobao.com/sug?${str}`)
      .then((response) => response.json())
      .then((d) => {
        if (currentValue === value) {
          const result = d.result;
          const data = [];
          result.forEach((r) => {
            data.push({
              value: r[0],
              text: r[0],
            });
          });
          callback(data);
        }
      });
  }

  timeout = setTimeout(fake, 300);
}
</script>

<style>
</style>