<template>
    <div class="container mt-custom">
        <div class="row">
            <div class="col-md-12">
                <div class="card border-0 rounded shadow">
                    <div class="card-body">
                        <h4>DATA POST</h4>
                        <hr>
                        <router-link :to="{ name: 'posts.create' }" class="btn btn-md btn-success">TAMBAH POST</router-link>

                        <table class="table table-striped table-bordered mt-4">
                            <thead class="thead-dark">
                                <tr>
                                    <th scope="col">id</th>
                                    <th scope="col">nama</th>
                                    <th scope="col">jurusan</th>
                                    <th scope="col">hobi</th>
                                    <th scope="col">OPTIONS</th>
                                </tr>
                            </thead>
                            <tbody>
                               <tr v-for="(siswa, index) in siswaList.data" :key="index">
                                    <td>{{ index + 1 }}</td>
                                    <td>{{ siswa.nama }}</td>
                                    <td>{{ siswa.jurusan }}</td>
                                    <td>{{ siswa.hobi }}</td>
                                    <td class="text-center">
                                        <router-link :to="{name: 'posts.edit', params:{id: siswa.id }}" class="btn btn-sm btn-primary me-2">EDIT</router-link>
                                        <button @click.prevent="postDelete(siswa.id)" class="btn btn-sm btn-danger ml-1">DELETE</button>
                                    </td>
                                </tr>
                            </tbody>
                        </table>

                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios';

export default {
    name: 'IndexPosts',
    data() {
        return {
            siswaList: [], // Mengubah variabel menjadi siswaList
        }
    },
    methods: {
        postDelete(id) {
            axios.delete(`http://127.0.0.1:3000/delete/${id}`)
                .then(response => {
                    console.log('Berhasil menghapus siswa dengan id', id ,response);
                    // reload
                    location.reload()
                })
                .catch(error => {
                    console.error('Gagal menghapus siswa:', error);
                });
        },
        setSiswaList(data) {
            this.siswaList = data;
        }
    },
    mounted() {
        axios.get('http://127.0.0.1:3000/tampil')
            .then((response) => this.setSiswaList(response.data))
            .catch((error) => console.log("gagal", error))
    }


}
</script>
