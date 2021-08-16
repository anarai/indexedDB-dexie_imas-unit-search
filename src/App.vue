<template>
  <div id="app">
    <h2>im@s unit search</h2>
    <search-form @searchFilter="searchFilter"/>
    <p>{{ message }}</p>
    <item-table :disp-data="dispData"/>
  </div>
</template>

<script>
import jsonData from "./assets/imas_unit.json";
import itemTable from "./component/itemTable.vue";
import searchForm from "./component/searchForm.vue";
import Dexie from "dexie";

export default {
  name: "App",
  data() {
    return {
      dbName: "imasDB",
      db: "",
      dispData: [],
      json: jsonData,
      message: ""
    };
  },
  components: {
    itemTable,
    searchForm
  },
  mounted() {
    this.setInitDB();
  },
  methods: {
    async setInitDB() {
      try {
        // DB open
        this.db = new Dexie(this.dbName);
        this.db.version(1).stores({
          unitTable: "++id, unit_name, member"
        });
        await this.db.open();

        let tableItemCount = await this.db.unitTable.count();

        // first access data insert
        if (tableItemCount === 0) {
          await this.db.unitTable.bulkAdd(this.json);
          tableItemCount = await this.db.unitTable.count();
        }

        this.dispData = await this.db.unitTable.orderBy("id").toArray();
        this.message = `初回全件表示: ${tableItemCount}件`;
      } catch (e) {
        this.message += `>db open error:${e}<br />`;
      }
    },
    async searchFilter(filterInfo) {
      let beginTime = new Date().getTime();
      const filterResult = await this.db.unitTable.filter(v => {
        return v[filterInfo.target].includes(filterInfo.word);
      });

      const resultCount = await filterResult.count();
      this.dispData = await filterResult.toArray();

      const dispResultTime = (new Date().getTime() - beginTime) / 1000;
      this.message = `検索結果: ${resultCount}件 ${dispResultTime}s`;
    }
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
