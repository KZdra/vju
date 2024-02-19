<template>
  <div class="container mt-custom">
    <div class="row">
      <div class="col-md-12">
        <div class="card border-0 rounded shadow">
          <div class="card-body">
            <h4>TAMBAH POST</h4>
            <hr />
            <div v-if="validation.errors" class="mt-2 alert alert-danger">
              <ul class="mt-0 mb-0">
                <li v-for="(error, index) in validation.errors" :key="index">
                  {{ `${error.param} : ${error.msg}` }}
                </li>
              </ul>
            </div>
            <form @submit.prevent="store">
              <div class="form-group">
                <label for="nama" class="font-weight-bold mb-2">Nama</label>
                <input
                  type="text"
                  class="form-control"
                  v-model="post.nama"
                  placeholder="Masukkan Nama"
                />
              </div>
              <div class="form-group mt-3">
                <label for="content" class="font-weight-bold mb-2"
                  >Jurusan</label
                >
                <input
                  type="text"
                  class="form-control"
                  v-model="post.jurusan"
                  placeholder="Masukkan Jurusan"
                />
              </div>
              <div class="form-group mt-3">
                <label for="content" class="font-weight-bold mb-2">Hobi</label>
                <input
                  type="text"
                  class="form-control"
                  v-model="post.hobi"
                  placeholder="Masukkan Hobi"
                />
              </div>
              <button type="submit" class="btn btn-primary mt-3">SIMPAN</button>
            </form>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { reactive, ref } from "vue";
import { useRouter } from "vue-router";
import axios from "axios";

export default {
  name: "CreatePage",
  setup() {
    //state posts
    const post = reactive({
      nama: "",
      jurusan: "",
      hobi: "",
    });

    //state validation
    const validation = ref([]);

    //vue router
    const router = useRouter();

    //method store
    function store() {
      let nama = post.nama;
      let jurusan = post.jurusan;
      let hobi = post.hobi;

      axios
        .post("http://localhost:3000/tambah", {
          nama: nama,
          jurusan: jurusan,
          hobi: hobi,
        })
        .then(() => {
          //redirect ke post index
          router.push({
            name: "posts.index",
          });
        })
        .catch((error) => {
          //assign state validation with error
          validation.value = error.response.data;
        });
    }

    //return
    return {
      post,
      validation,
      router,
      store,
    };
  },
};
</script>
