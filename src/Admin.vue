<template>
<div class="row">
    <div class="col-12  mt-5 pt-3 pb-3 bg-white from-wrapper">
        <h3>Tambah Data Mahasiswa Baru</h3>
        <hr>
        <form @submit.prevent="save()">
          <div class="row">
            <div class="col-12 col-sm-6">
              <div class="form-group">
               <label for="nim">NIM</label>
               <input type="text" class="form-control" name="nim" id="nim" v-model="nim" />
              </div>
            </div>
            <div class="col-12 ">
              <div class="form-group">
               <label for="nama">Nama</label>
               <input type="text" class="form-control" name="nama" id="nama" v-model="nama" />
              </div>
            </div>
            <div class="col-12">
              <div class="form-group">
               <label for="alamat">Alamat</label>
               <input type="text" class="form-control" name="alamat" id="alamat" v-model="alamat" />
              </div>
            </div>
            <div class="col-12 col-sm-6">
              <div class="form-group">
               <label for="nohp">Nomor HP</label>
               <input type="nohp" class="form-control" name="nohp" id="nohp" v-model="nohp" value />
             </div>
           </div>
           <div class="col-12 col-sm-6">
             <div class="form-group">
              <label for="email">Email</label>
              <input type="email" class="form-control" name="email" id="email" v-model="email" />
            </div>
          </div>
           <div class="col-4">
              <div class="form-group">
               <label for="indeks">Indeks</label>
               <input type="indeks" class="form-control" name="indeks" id="indeks" v-model="indeks" value />
             </div>
           </div>
           
          <alert v-if="msg" :msg="msg" :classAlert="classAlert"></alert>
          </div>

          <div class="row">
            <div class="col-12 ">
              <button type="submit" class="btn btn-primary">{{(id ? 'Update' : 'Create')}}</button>
              <button @click.prevent="cancel()" class="btn btn-secondary float-right">Cancel</button>
            </div>
          </div>
        </form>
      </div>
      <div v-if= "posts.length > 0" class="col-12  mt-5 pt-3 pb-3 bg-white from-wrapper">
        <h3>Data Mahasiswa</h3>
        <hr>
        <PostItem @delete="deletePost" @edit="getPost" v-for="(post, index) in posts" :key="post.id" :index="index" :post="post"></PostItem>
      </div>
    </div>
</template>

<script>
import Alert from "./cmps/Alert";
import PostItem from "./cmps/PostItem";
export default {
    data () {
        return {
            msg:null,
            classAlert: null,
            id: null,
            post_index: null,
            posts: [],
            nim: "",
            nama: "",
            alamat: "",
            nohp: "",
            email: "",
            indeks: ""
        };
    },
    components : {
        Alert,
        PostItem
    },
      methods: {
        save() {
          if(this.id){
            this.update();
          }else {
            this.create();
          }
        },
        create () {
          const form = new FormData();
          form.append("nim", this.nim);
          form.append("nama", this.nama);
          form.append("alamat", this.alamat);
          form.append("nohp", this.nohp);
          form.append("email", this.email);
          form.append("indeks", this.indeks);

          //axios
          this.$api
          .post("/mahasiswa", form)
          .then(res => {
            this.msg = "Mahasiswa Baru berhasil Ditambahkan!";
            this.classAlert = "success";
            this.posts.unshift(res.data);
            this.cancel(true);
          })
          .catch(err => {
            this.msg = err.response.data.messages.error;
            this.classAlert = "danger";
          });
        },
        update() {
          const form = new FormData();
          form.append("nim", this.nim);
          form.append("nama", this.nama);
          form.append("alamat", this.alamat);
          form.append("nohp", this.nohp);
          form.append("email", this.email);
          form.append("indeks", this.indeks);

          //axios
          this.$api
            .post("/mahasiswa/update/" + this.id, form)
            .then(res => {
              this.msg = "Data Mahasiswa telah diperbaharui";
              this.classAlert = "success";
              this.posts[this.post_index] = res.data;
              this.cancel(true);
            })
            .catch(err => {
              this.msg = err.response.data.messages.error;
              this.classAlert = "danger";
            });
        },
        get(){
            this.$api.get('/mahasiswa')
            .then(res => {
                this.posts = res.data
            })
            .catch(err =>{
                console.log(err.response);

                this.msg = err.response.data.messages.error;
                this.classAlert = "danger";
            })
        },
        getPost(id, post_index){
          this.cancel()
          this.id = id;
          this.post_index = post_index;

          this.$api.get("/mahasiswa/" + id).then(res => {
            this.nim = res.data.nim;
            this.nama = res.data.nama;
            this.alamat = res.data.alamat;
            this.nohp = res.data.nohp;
            this.email = res.data.email;
            this.indeks = res.data.indeks;
        
            window.scroll(0, 0);
        });
        },
        deletePost(id, index){
          this.$api
          .delete("/mahasiswa/" + id)
          .then(() => {
            this.msg = "Data mahasiswa berhasil dihapus";
            this.classAlert = "warning";
            this.cancel(true);
            this.posts.splice(index, 1);
            window.scroll(0, 0);
          })
          .catch(err => {
            this.msg = err.response.data.messages.error;
            this.classAlert = "danger";
          });
        },
        cancel(alertShow = false){
          this.post_index = null;
          this.id = null;
          this.nim = "";
          this.nama = "";
          this.alamat = "";
          this.nohp = "";
          this.email = "";
          this.indeks = "";
          if(!alertShow){
            this.msg = null;
            this.classAlert = null;
        }
      }
    }, 
    created() {
        this.get();
    }
};
</script>

<style>

</style>