<template>
<div class="app">
    <h1 style="text-align: center;">Selamat Datang Admin...</h1>
<div class="pr">
    <div class="border1 row">
  <h1>Form Peminjaman Buku</h1>

      <form @submit.prevent="add">
        <input type="hidden" v-model="form.id">
        <label for=""><b> Nama Siswa : </b></label><br>
        <input type="text" v-model="form.namaSiswa"><br><br>
        <label for=""><b> Judul Buku : </b></label><br>
        <input type="text" v-model="form.judulBuku"><br><br>
        <label for=""><b> Tanggal Pinjam : </b></label><br>
        <input type="date" v-model="form.tanggalPinjam"><br><br>
        <label for=""><b> Tanggal Peminjaman : </b></label><br>
        <input type="date" v-model="form.tanggalPengembalian"><br><br>
        <button type="submit" v-show="!updateSubmit" style="margin: auto;"><b> Add Peminjaman </b></button> <!-- jika tidak update maka tombol update tidak muncul --> 
        <button type="button" v-show="updateSubmit" @click="update(form)"><b> Update </b></button> <!-- jika tombol edit di klik maka tombol add akan berubah menjadi update -->
    </form><br>
    </div>

    <div class="border2">
    <h1>Tabel Daftar Buku</h1>
    <div id="app1" class="row">
      <b><label for="quantity">Show</label> 
      <input type="number" id="quantity" name="quantity" min="0" max="50"> entries</b>
    </div>

    <div id="app2">
      <b><label for="quantity">Search</label> 
      <input type="text" id="quantity" name="quantity" min="0" max="50"></b>
    </div>

    <table>
            <thead>
              <tr>
                <th>No</th>
                <th>Nama Siswa</th>
                <th>Judul Buku</th>
                <th>Tanggal Pinjam</th>
                <th>Tanggal Pengembalian</th>
                <th>Aksi</th>
              </tr>
            </thead>
            <tbody v-for="user in users" :key="user.id">
                <tr>
                <td>{{ user.id+1 }}</td>
                <td>{{ user.namaSiswa }}</td>
                <td>{{ user.judulBuku }}</td>
                <td>{{ user.tanggalPinjam }}</td>
                <td>{{ user.tanggalPengembalian }}</td>
                <td><button style="background-color: green; color: white; padding: 5px; border: 1px solid white;" @click="perpanjang(user)">Perpanjang</button><br>
                <button style="background-color: red; color: white; width: 80px; padding: 5px;  border: 1px solid white;" @click="kembali(user)">Kembali</button></td>
              </tr>
            </tbody>
  </table>

  <p>Showing 1 to 1 of 1 entries</p>
  </div>
  </div>

  </div>
</template>

<script>
/* eslint-disable */ 
import axios from 'axios'
export default {
  data(){
    return{
        form: {
          id: '',
          namaSiswa: '',
          judulBuku: '',
          tanggalPinjam: '',
          tanggalPengembalian: ''
        },
        users: '',
        updateSubmit: false
    }
  },
  mounted() {
    this.load()
  },
  methods: {
    load(){
        axios.get('http://localhost:3000/users').then(res => {
        this.users = res.data
      }).catch ((err) => {
        console.log(err);
        
      })
    },
      add(){
      axios.post('http://localhost:3000/users/', this.form).then(res => {
          this.load()
          this.form.id = '',
          this.form.namaSiswa = '',
          this.form.judulBuku = '',
          this.form.tanggalPinjam = '',
          this.form.tanggalPengembalian = ''

      })
    },
    perpanjang(user){ 
        this.updateSubmit = true
        this.form.id = user.id 
        this.form.namaSiswa = user.namaSiswa 
        this.form.judulBuku = user.judulBuku 
        this.form.tanggalPinjam = user.tanggalPinjam 
        this.form.tanggalPengembalian = user.tanggalPengembalian 
    },
    update(form){ 
       return axios.put('http://localhost:3000/users/' + form.id , {namaSiswa: this.form.namaSiswa, judulBuku: this.form.judulBuku, tanggalPinjam: this.form.tanggalPinjam, tanggalPengembalian: this.form.tanggalPengembalian}).then(res => {
        this.load()
        this.form.id = ''
        this.form.namaSiswa = ''
        this.form.judulBuku = ''
        this.form.tanggalPinjam = ''
        this.form.tanggalPengembalian = ''
        this.updateSubmit = false
      }).catch((err) => {
        console.log(err);
        
      })
    },
    kembali(user){
      axios.delete('http://localhost:3000/users/' + user.id).then(res =>{
          this.load()
          let index = this.users.indexOf(form.namaSiswa)
          this.users.splice(index,1)
      })
    }
  }
}
</script>

<style scoped>
.app {
border: 1px solid black none;
background-color: lightblue;
padding: 10px;
margin: 40px;
}

.pr {
display: flex;}

.border1 {
width: 30%;
padding: 30px;
background-color: none;
}

.border2 {
width: 40%;
padding: 30px;
}

.row:after {
  content: "";
  display: table;
  clear: both;
}

table {
  border: 1px solid white ;
  padding: 20px;
  width: 700px;
  border-collapse: collapse;
  background-color: white;
}

th {
border: 1px solid white ;
height: 70px;
background-color: lightblue;
}

td {
border: 1px solid black none;
text-align: center;
}

#app1 {
float: left;
width: 30%;
padding: 10px;
}

#app2 {
float: right;
width: 40%;
padding: 10px;
}

input, select {
  width: 100%;
  padding: 12px 20px;
  margin: 8px 0;
  display: inline-block;
  border: 1px solid #ccc;
  border-radius: 4px;
  box-sizing: border-box;
}

button[type=submit] {
  width: 100%;
  background-color: #00bfff	;
  color: white;
  padding: 14px 20px;
  margin: 8px 0;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

button[type=button] {
  width: 100%;
  background-color: #00bfff	;
  color: white;
  padding: 14px 20px;
  margin: 8px 0;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}


</style>
