<template>
  <v-container fluid style="padding: 30px">
    <div class="my-4">
      <v-toolbar-title class="font-weight-medium">
        <b>EDIT ADMINISTRATOR</b>
      </v-toolbar-title>
    </div>
    <v-card class="rounded-xl" height="320px" color="#ffffed">
    <v-form>
      <v-container>
        <p>Nama</p>
        <v-text-field class="form" v-model="nama" label="Masukkan Nama" solo></v-text-field><br>
        <p>NIP</p>
        <v-text-field class="form" v-model="nip" label="Masukkan NIP" solo></v-text-field><br>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn large class="btn white--text" color="#DB5248" href="/Administrator"> batal </v-btn>
          <v-btn large right class="btn mr-4 white--text" @click="submit" color="#0D987A"> Simpan </v-btn>
        </v-card-actions>
      </v-container>
    </v-form>
    </v-card>
  </v-container>
</template>

<script>
export default {
  name: "App",
  data: () => ({
    nama: "",
    nip: "",
    sip: "",
    alamat: "",
    tgl: "",
    pk: "",
    email: "",
    pw: "",
    cpw: "",
    row: null,
        show1: false,
        show2: true,
        show3: false,
        show4: false,
        password: 'Password',
        rules: {
          required: value => !!value || 'Required.',
          emailMatch: () => (`The email and password you entered don't match`),
        },
      date: (new Date(Date.now() - (new Date()).getTimezoneOffset() * 60000)).toISOString().substr(0, 10),
      menu: false,
      modal: false,
      menu2: false,
  }),
};
</script>

<style>
.btn {
  height: 80px;
  width: 135px;
  border-radius: 15px;
}
.form {
  border-style: ridge;
  border-color: #0B715B;
  border-width: 1.5px;
  border-radius: 10px;
  height: 51px;
}
</style>