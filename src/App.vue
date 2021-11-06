<template>
  <div id="q">
    <br />
    <marquee direction="up" style="text-align: center" id="e">
      <h2>Selamat Datang Diperpustakan Online</h2>
      <h2>Silahkan Membaca atau Meminjam Juga boleh</h2>
      <h2>Dengan Syarat Mengisi Ketentetuan dibawah</h2>
    </marquee>
    <div class="b">
      <form @submit.prevent="add" action="login.php" method="POST" id="g">
        <h3>Form Peminjaman Buku</h3>
        <p>
          <label><b>Nama Siswa:</b></label
          ><br />
          <input
            placeholder="Masukkan Nama..."
            type="text"
            v-model="form.nama"
            required
          />
        </p>
        <p>
          <label><b>Judul Buku:</b></label
          ><br />
          <input
            placeholder="Judul Buku..."
            type="text"
            v-model="form.judulbuku"
            required
          />
        </p>
        <p>
          <label><b>Tanggal Pinjam:</b></label
          ><br />
          <input
            placeholder="Tanggal Pinjam..."
            type="text"
            v-model="form.tanggalpinjam"
            required
          />
        </p>
        <p>
          <label><b>Tanggal Pengembalian:</b></label
          ><br />
          <input
            placeholder="Tanggal Pengembalian..."
            type="text"
            v-model="form.tanggalpengembalian"
            required
          />
        </p>
        <button
          type="submit"
          style="width: 210px"
          id="f"
          v-show="!updateSubmit"
        >
          Add Pinjaman
        </button>
        <button
          type="submit"
          style="width: 210px"
          id="f"
          v-show="updateSubmit"
          @click="update(form)"
        >
          Update
        </button>
      </form>
      <div class="c">
        <h3>Data Peminjam Buku</h3>
        <table class="table" id="d">
          <thead class="thead-dark">
            <tr>
              <th>No</th>
              <th>Nama Siswa</th>
              <th>Judul Buku</th>
              <th>Tanggal Pinjam</th>
              <th>Tanggal Pengembalian</th>
              <th>Action</th>
            </tr>
          </thead>
          <tbody class="table-light">
            <tr v-for="user in Project4" :key="user.id">
              <td scope="row">
                <b>{{ user.id }}</b>
              </td>
              <td>{{ user.nama }}</td>
              <td>{{ user.judulbuku }}</td>
              <td>
                {{ user.tanggalpinjam }}
              </td>
              <td>
                {{ user.tanggalpengembalian }}
              </td>
              <td>
                <button id="z" class="btn btn-success" @click="edit(user)">
                  Perpanjangan
                </button>
                <button class="btn btn-danger" @click="del(user)">
                  Pengembalian
                </button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
/* eslint-disable */
import axios from "axios";
export default {
  data() {
    return {
      form: {
        id: "",
        nama: "",
        judulbuku: "",
        tanggalpinjam: "",
        tanggalpengembalian: "",
      },
      Project4: "",
      updateSubmit: false,
    };
  },
  mounted() {
    this.load();
  },
  methods: {
    load() {
      axios
        .get("http://localhost:3000/Project4")
        .then((res) => {
          this.Project4 = res.data; //respon dari rest api dimasukan ke users
        })
        .catch((err) => {
          console.log(err);
        });
    },
    add() {
      axios.post("http://localhost:3000/Project4", this.form).then((res) => {
        this.load();
        this.form.nama = "";
        this.form.judulbuku = "";
        this.form.tanggalpinjam = "";
        this.form.tanggalpengembalian = "";
      });
    },
    edit(user) {
      this.updateSubmit = true;
      this.form.id = user.id;
      this.form.nama = user.nama;
      this.form.judulbuku = user.judulbuku;
      this.form.tanggalpinjam = user.tanggalpinjam;
      this.form.tanggalpengembalian = user.tanggalpengembalian;
    },
    update(form) {
      return axios
        .put("http://localhost:3000/Project4/" + form.id, {
          nama: this.form.nama,
          judulbuku: this.form.judulbuku,
          tanggalpinjam: this.form.tanggalpinjam,
          tanggalpengembalian: this.form.tanggalpengembalian,
        })
        .then((res) => {
          this.load();
          this.form.id = "";
          this.form.nama = "";
          this.form.judulbuku = "";
          this.form.tanggalpinjam = "";
          this.form.tanggalpengembalian = "";
          this.updateSubmit = false;
        })
        .catch((err) => {
          console.log(err);
        });
    },
    del(user) {
      axios.delete("http://localhost:3000/Project4/" + user.id).then((res) => {
        this.load();
        let index = this.Project4.indexOf(form.nama);
        this.Project4.splice(index, 1);
      });
    },
  },
};
</script>

<style>
#a {
  background-color: rgb(63, 201, 255);
  font-family: "Courier New";
}
#q {
  margin-left: 50px;
  margin-right: 50px;
  margin-bottom: 30px;
  margin-top: 30px;
  padding-top: 30px;
  padding-bottom: 30px;
  border-radius: 20px;
  background-color: rgb(83, 180, 255);
}
.b {
  display: flex;
  text-align: left;
  padding-left: 30px;
}
.c {
  width: 900px;
  margin-left: 40px;
  margin-right: 40px;
  padding-top: 20px;
  text-align: center;
}
#f {
  background-color: rgb(60, 171, 255);
}
#e {
  border: 2px dashed black;
  text-align: center;
  border-radius: 35px;
  margin-right: 150px;
  margin-left: 150px;
  margin-top: 150;
}
#g {
  border: 2px dashed black;
  text-align: center;
  padding: 15px;
  border-radius: 35px;
  margin-left: 120px;
  margin-top: 150;
}
#z {
  margin-bottom: 5px;
}
</style>
