<template>
  <div v-if="post" class="col-12  mt-5 pt-3 pb-3 bg-white from-wrapper">
      <div class = "col-12 col-sm-8">
          <h6>NIM: {{post.nim}}</h6>
          <h6>Nama: {{post.nama}}</h6>
          <h6>Alamat: {{post.alamat}}</h6>
          <h6>Nomor HP: {{post.nohp}}</h6>
          <h6>Email: {{post.email}}</h6>
          <h6>Indeks: {{post.indeks}}</h6>
      </div>
      <div class="col-12 mt-3">
          <router-link class="btn btn-outline-success mr-2" to="/">Semua Mahasiswa</router-link>
      </div>
  </div>
</template>

<script>
export default {
    data(){
      return {
        post: false
      };
    },
    methods: {
      get(){
        var id = this.$route.params.id;
        this.$api.get("/mahasiswa/" + id)
        .then(res => {
          this.post = res.data;
        })
        .catch(err =>{
          console.log(err.response);
        });
      }
    },
    created(){
      this.get();
    }
};
</script>

<style>

</style>