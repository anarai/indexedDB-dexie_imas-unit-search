<template>
  <table id="itemTable">
    <tr>
      <th>ユニット名</th>
      <th>メンバー</th>
    </tr>
    <tr v-for="item in dispData" :key="item.id">
      <td>{{item.unit_name}}</td>
      <td>{{item.member}}</td>
    </tr>
  </table>
</template>

<script>
export default {
  props: {
    dispData: {
      type: Array,
      required: false,
      default: () => []
    }
  }
};
</script>

<style>
#itemTable {
  border-collapse: collapse;
  border-spacing: 0;
  width: 100%;
  text-align: left;
}
#itemTable tr {
  border-bottom: solid 1px #ccc;
}
#itemTable th,
#itemTable td {
  padding: 15px 5px;
}
</style>
