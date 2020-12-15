<template>
  <div class="row post mb-1">
      <div class = "col-12 col-sm-8">
          <h6>NIM: {{post.nim}}</h6>
          <h6>Nama: {{post.nama}}</h6>
          <h6>Alamat: {{post.alamat}}</h6>
          <h6>Nomor HP: {{post.nohp}}</h6>
          <h6>Email: {{post.email}}</h6>
          <h6>Indeks: {{post.indeks}}</h6>
      </div>
      <div class="col-12 mt-3">
          <router-link class="btn btn-outline-success mr-2" :to="'/post/' + post.id">View</router-link>
          <button class="btn btn-outline-primary" @click="$emit('edit', post.id, index)">Edit</button>
          <button class="btn btn-outline-danger float-right" @click="$emit('delete', post.id, index)">Delete</button>
      </div>
      <div class="col-12">
          <hr>
      </div>
  </div>
</template>

<script>
export default {
    props: ["post", "index"]
};
</script>

<style>

</style>