<template>
  <div class="greetings">
    <h1 class="green">{{ msg }}</h1>
  </div>
  <div>
    <p>Hastane Kayit</p>
    <form @submit="postData" method="post">
      <div>
        <label for="hospital">
          Hastane adi:
        </label>
        <input type="text" id="hospitalname" v-model="posts.name">
      </div>
      <div>
        <label for="address">
          Addres:
        </label>
        <input type="text" id="address" v-model="posts.address">
      </div>
      <div>
        <button type="submit">Save</button>
      </div>
    </form>
  </div>

  <div>
    <table>
      <thead>
        <tr>
          <th>sira</th>
          <th>Isim</th>
          <th>Adress</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(item, index)  in apiData" :key="item.id">
          <td>{{ index + 1 }}</td>
          <td>{{ item.name }}</td>
          <td>{{ item.address }}</td>
        </tr>
      </tbody>
    </table>
  </div>
  <div>
  </div>
</template>



<script>
import axios from 'axios';
//  axios.defaults.headers.common['Access-Control-Allow-Origin'] = '*';
export default {
  name: "PostComponent",
  data() {
    return {
      posts: {
        name: null,
        address: null
      },
      apiData: []
    }
  },
  mounted() {
    this.getData();
  },
  methods: {
    postData(e) {
      console.log(axios.defaults.headers)
      axios.post("http://localhost:5179/api/hospital", this.posts)
        .then(res => {
            this.apiData.push(res.data)
        })
      e.preventDefault();
    },
    getData() {
      axios.get('http://localhost:5179/api/hospital').then((response) => { this.apiData = response.data; }).catch((error) => { console.error('Error: ', error); });
    }
  }
}
</script>