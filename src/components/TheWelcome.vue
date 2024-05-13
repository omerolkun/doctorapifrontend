<template>
  <div class="doctor">
    <div>
      <p>Doktor kayit</p>
      <form @submit="postData" method="post">
        <div>
          <label for="doctor">
            Doktor Adi:
          </label>
          <input type="text" id="doctorname" v-model="posts.name">
        </div>
        <div>
          <label for="doctorsurname">
            Doktor soyadi:
          </label>
          <input type="text" id="doctorsurname" v-model="posts.surname">
        </div>
        <div>
          <label for="doctortckn">
            Doktor TCKN:
          </label>
          <input type="text" id="tckn" v-model="posts.tckn">
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
            <th>soyad</th>
            <th>tckn</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(item, index)  in apiData" :key="item.id">
            <td>{{ index + 1 }}</td>
            <td>{{ item.name }}</td>
            <td>{{ item.surname}}</td>
            <td>{{ item.tckn }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  name: "PostComponent",
  data() {
    return {
      posts: {
        name: null,
        surname: null,
        tckn: null
      },
      apiData: []
    }
  },
  mounted(){
    this.getData();
  },
  methods: {
    postData(e) {
      console.log(axios.defaults.headers)
      axios.post("http://localhost:5179/api/doctor", this.posts)
      .then(res=> {
        this.apiData.push(res.data)
      })
      e.preventDefault();
    },
    getData(){
      axios.get('http://localhost:5179/api/doctor').then((response) => {this.apiData = response.data;}).catch((error)=> {console.error('Error' ,error);})
    }
  }
}
</script>