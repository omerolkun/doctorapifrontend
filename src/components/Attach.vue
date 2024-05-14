<template>
    <div class="final">
        <p>Doktor ve hastanesi</p>
        <form @submit="postData" method="post">
            <div>
                <label for="doctor">Doktor TCKN: </label>
                <input type="text" id="doctor" name="doctor" v-model="posts.doctorid">
            </div>
            <div>
                <label for="hospital">hastanesi</label>
                <select id="hospital" name="hospital" v-model="posts.hospitalid">
                    <option v-for="item in apiData" v-bind:value="item.id">
                        {{ item.name }}
                    </option>
                </select>
            </div>
            <div>
                <button type="submit">Save</button>
            </div>

            <div>
                <table>
                    <thead>
                        <tr>
                            <th>sira</th>
                            <th>doctorId</th>
                            <th>hospitalId</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="(item,index) in junctionTableData" :key="item.id">
                            <td>{{ index+ 1 }}</td>
                            <td>{{ item.doctorId }}</td>
                            <td>{{ item.hospitalId }}</td>
                        </tr>
                    </tbody>
                </table>
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
                doctorid: null,
                hospitalid: null
            },
            apiData: [],
            junctionTableData: []
        }
    },
    mounted() {
        this.getHospitalData();
        this.getJunctionTableData();
    },
    methods: {
        getHospitalData() {
            axios.get('http://localhost:5179/api/hospital')
                .then((response) => {
                    this.apiData = response.data;
                })
                .catch((error) => { console.error('Error: ', error); });
        },
        getJunctionTableData() {
            axios.get('http://localhost:5179/api/doctorhospital')
                .then((response) => {
                    this.junctionTableData= response.data;
               //     console.log(junctionTableData);
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