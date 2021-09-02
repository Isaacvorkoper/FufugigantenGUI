<template>
  <div>
    <h2>Manufacturer List</h2>

    <table id="MyTable" class="table-fixed">
      <tr>
        <th>ID</th>
        <th class="w-1/2">Manufacturer</th>
        <th>Edit</th>
        <th>Delete</th>
      </tr>
      <MyTable v-for="manufacturer in manufacturers" :key="manufacturer.manufacturerId" :manufacturer="manufacturer"/>
    </table>
  </div>
</template>

<script>
import MyTable from "../components/MyTable"
export default {
  components: {
    MyTable
  },
  data() {
    return {
      manufacturers: []
    }
  },
  head() {
    return {
      title: "Manufacturer List",
      meta: [
        {
          hid: "description",
          name: "description",
          content: "Fufugiganten data"
        }
      ]
    };
  },
  async created() {
    const config = {
      headers: {
        'Accept': 'application/json'
      }
    }
    try {
      const res = await this.$axios.get('https://localhost:44319/api/Manufacturers', config);
      console.log(res.data)
      this.manufacturers = res.data;
    } catch (err) {
      console.log(err)
    }
  }
};
</script>
