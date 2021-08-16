<template>
  <div id="searchForm">
    <select v-model="filterInfo.target">
      <option value="unit_name">ユニット名で検索</option>
      <option value="member">アイドル名で検索</option>
    </select>
    <input type="text" placeholder="例：竜宮小町" v-model="filterInfo.word">
    <button @click="setFilter">検索</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      filterInfo: {
        target: "unit_name",
        word: ""
      }
    };
  },
  methods: {
    setFilter() {
      this.$emit("searchFilter", this.filterInfo);
    }
  }
};
</script>

<style>
select,
input {
  margin-right: 5px;
}
</style>
