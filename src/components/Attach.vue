<template>
    <div class="final">
        <p>Doktor ve hastanesi</p>
        <form @submit="postData" method="post">
            <div>
                <label for="doctor">Doktor TCKN: </label>
                <input type="text" id="doctor" name="doctor" v-model="posts.doctor">
            </div>
            <div>
                <label for="hospital">hastanesi</label>
                <select id="hospital" name="hospital" v-model="posts.hospital">
                    <option value="item.id" v-for="item in apiData" :key="item.id">{{ item.name}} </option>
                </select>
            </div>
            <div>
                <button type="submit">Save</button>
            </div>

        </form>

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
                doctor: null,
                hospital: null
            },
            apiData: []
        }
    },
    mounted() {
        this.getHospitalData();
    },
    methods: {
        getHospitalData() {
            axios.get('http://localhost:5179/api/hospital')
                .then((response) => {
                    this.apiData = response.data;
                })
                .catch((error) => { console.error('Error: ', error); });
        },
        postData(e) {
            //console.log(axios.defaults.headers)
            console.log(this.posts);
            axios.post("http://localhost:5179/api/doctorhospital", this.posts)
                .then(res => {
                    this.apiData.push(res.data)
                })
            e.preventDefault();
        },
    }
}
</script>