<template>
  <div id="app" class="container my-3">
    <h3>Vue Fetch example</h3>

    <div class="card mt-3">
      <div class="card-header">Vue Fetch GET</div>
      <div class="card-body">
        <div class="input-group input-group-sm">
          <input type="text" ref="get_id" class="form-control ml-2" placeholder="Id" />
          <div class="input-group-append">
            <button class="btn btn-sm btn-primary" @click="getDataById">Get by Id</button>
          </div>  
        
        <div v-if="getResult" class="alert alert-secondary mt-2" role="alert"><pre>
          <!-- {{getResult}} -->
          <table>
              <tr>
                  <td>Name:</td>
                  <td>{{getResult.name}}</td>
              </tr>
              <tr>
                  <td>Description:</td>
                  <td>{{getResult.description}}</td>
              </tr>
              <tr>
                  <td>Attributes:</td>
                  <td>
                      <div v-for="attr in getResult.attributes" :key="attr">{{ attr }}</div>
                  </td>
              </tr>
              </table>
          </pre></div>
      </div>
    </div>
  </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      getResult: null
    }
  },
  methods: {
    fortmatResponse(res) {
      return JSON.stringify(res, null, 2);
    },
    async getDataById() {
      const id = this.$refs.get_id.value;
      if (id) {
        try {
          axios.get(`http://localhost:8081/apiobj/${id}`)
          .then((response) => {
            console.log(response.data);
            this.getResult = response.data;
            })
          .catch((err) => console.log(err))
          // const res = await http.get(`/apiobj/${id}`);
          // const result = {
          //   status: res.status + "-" + res.statusText,
          //   headers: res.headers,
          //   data: res.data,
          // };
          // const result = {
          //   data: res.data,
          //   status: res.status,
          //   statusText: res.statusText,
          //   headers: res.headers,
          //   config: res.config,
          // };
          //this.getResult = this.fortmatResponse(result);
        } catch (err) {
          this.getResult = this.fortmatResponse(err.response?.data) || err;
        }
      }
    }
  }
}
</script>

<style>
  #app {
    max-width: 600px;
    margin: auto;
  }
</style>
