<template>
  <v-container fluid style="padding: 30px">
    <br />
    <v-card class="rounded-xl" height="700px" color="#ffffed">
      <br />
      <v-card-title>
        <div>
          <br />
        </div>
        <v-btn
          class="white--text"
          href="/AddAntrean"
          height="53px"
          color="#613A03"
          left
        >
          <v-icon>mdi-plus </v-icon>
          <p class="my-5">Tambah Antrean</p>
        </v-btn>

        <v-spacer></v-spacer>
        <div class="text-right">
          <v-container>
            <v-btn class="white--text" height="50px" color="#613A03">
              <v-icon> mdi-filter-variant </v-icon>
            </v-btn>
          </v-container>
        </div>
        <div class="text-right">
          <v-text-field
            v-model.number="id"
            prepend-inner-icon="mdi-magnify"
            label="Search Here"
            solo
            class="form"
            hide-details
          ></v-text-field>
        </div>
      </v-card-title>

      <!-- code table -->
      <v-sheet :color="`white ${theme.isDark ? 'darken-2' : 'lighten-4'}`">
        <div>
          <v-sheet :color="`#ffffed`" class="px-5 py-0">
            <v-row>
              <v-col>
                <v-data-table
                  :headers="headers"
                  :items="queues"
                  :search="search"
                  hide-default-footer
                  :page.sync="page"
                  @page-count="pageCount = $event"
                  :items-per-page="itemsPerPage"
                  hide-default-header
                >
                  <template v-slot:header="{ props: { headers } }">
                    <thead class="MyHeader">
                      <tr color="#613A03">
                        <th
                          v-for="(h, index) in headers"
                          :class="h.class"
                          :key="index"
                        >
                          <span class="white--text JudulHeader">{{
                            h.text
                          }}</span>
                        </th>
                      </tr>
                    </thead>
                  </template>

                  <template v-slot:top>
                    <v-dialog v-model="dialogDelete" max-width="400px">
                      <v-card>
                        <v-card-title class="text-h5"
                          >Yakin ingin menghapus data ini?</v-card-title
                        >
                        <v-card-actions>
                          <v-spacer></v-spacer>
                          <v-btn color="primary" text @click="closeDelete"
                            >Cancel</v-btn
                          >
                          <v-btn color="primary" text @click="deleteItemConfirm"
                            >OK</v-btn
                          >
                          <v-spacer></v-spacer>
                        </v-card-actions>
                      </v-card>
                    </v-dialog>
                  </template>
                  <!-- buat nomor -->
                  <template v-slot:[`item.no`]="{ item }">
                    {{ nurses.indexOf(item) + 1 }}
                  </template>
                  <!-- atas ini buat nomor -->
                  <template v-slot:[`item.actions`]="{ item }">
                    <v-btn
                      class="aksi mx-2"
                      height="33px"
                      width="27px"
                      dark
                      x-small
                      href="/DetailPas"
                      color="#56CCF2"
                    >
                      <v-icon>mdi-card-account-details-outline</v-icon>
                    </v-btn>
                    <v-btn
                      class="aksi mx-2"
                      height="33px"
                      width="27px"
                      dark
                      x-small
                      href="/EditAntrean"
                      color="#F2994A"
                    >
                      <v-icon>mdi-pencil</v-icon>
                    </v-btn>
                    <v-btn
                      color="#F10000"
                      dark
                      class="aksi mx-2"
                      height="33px"
                      width="27px"
                      x-small
                      @click="deleteItem(item, item.id)"
                    >
                      <v-icon>mdi-close</v-icon>
                    </v-btn>
                  </template>
                </v-data-table>
              </v-col>
            </v-row>
          </v-sheet>
          <v-row color="#F9FFFB">
            <v-col>
              <div class="page d-flex justify-end mt-4">
                <v-sheet color="#F9FFFB" class="pa-5" :rounded="'lg'">
                  <template>
                    <div class="page">
                      <v-pagination
                        class="page"
                        color="#613A03"
                        v-model="page"
                        previous-aria-label="Prev"
                        next-aria-label="Next"
                        wrapper-aria-label
                        :length="10"
                        :total-visible="5"
                      ></v-pagination>
                    </div>
                  </template>
                </v-sheet>
              </div>
            </v-col>
          </v-row>
          <!-- <ul id="example-1">
                      <li v-for="data in queues" :key="data.index">
                        {{ data }}
                      </li>
                      </ul>
                      <button @click="getAllQueue">TEKAN INI WOYYY</button>
                      <p>{{id}}</p>
                      <p>{{token}}</p> -->
        </div>
      </v-sheet>
      <!-- bates code -->
    </v-card>

    <!-- If using vue-router -->
    <router-view></router-view>
  </v-container>
</template>

<script>
export default {
  name: "DataAdmin",
  inject: {
    theme: {
      default: { isDark: false },
    },
  },
  data() {
    return {
      totalPage: null,
      search: "",
      token: "",
      getid: null,
      id: null,
      waktu: new Date().toJSON().slice(0, 10),
      dialogDelete: false,
      selectedItemIndex: -1,
      pageCount: 0,
      itemsPerPage: 10,
      page: 1,
      queues: [],
      headers: [
        {
          text: "No.",
          value: "id",
        },
        {
          text: "Nama",
          value: "name",
        },
        {
          text: "NIK",
          value: "nik",
        },
        {
          text: "Poliklinik",
          value: "polyclinic.name",
        },
        {
          text: "Nomor Antrean",
          value: "queue_number",
        },
        {
          text: "Aksi",
          value: "actions",
        },
      ],
      identity: [
        {
          no: 1,
          name: "Beby Avilla",
          nik: "32016832687693",
          jenisKelamin: "Female",
          golonganDarah: "A",
          action: "edit",
        },
        {
          no: 2,
          name: "Beby Avilla",
          nik: "32016832687693",
          jenisKelamin: "Female",
          golonganDarah: "A",
          action: "edit",
        },
        {
          no: 3,
          name: "Beby Avilla",
          nik: "32016832687693",
          jenisKelamin: "Female",
          golonganDarah: "A",
          action: "edit",
        },
        {
          no: 4,
          name: "Beby Avilla",
          nik: "32016832687693",
          jenisKelamin: "Female",
          golonganDarah: "A",
          action: "edit",
        },
        {
          no: 5,
          name: "Beby Avilla",
          nik: "32016832687693",
          jenisKelamin: "Female",
          golonganDarah: "A",
          action: "edit",
        },
        {
          no: 6,
          name: "Beby Avilla",
          nik: "32016832687693",
          jenisKelamin: "Female",
          golonganDarah: "A",
          action: "edit",
        },
        {
          no: 7,
          name: "Beby Avilla",
          nik: "32016832687693",
          jenisKelamin: "Female",
          golonganDarah: "A",
          action: "edit",
        },
        {
          no: 8,
          name: "Beby Avilla",
          nik: "32016832687693",
          jenisKelamin: "Female",
          golonganDarah: "A",
          action: "edit",
        },
      ],
    };
  },
  methods: {
    add(Registrasi) {
      this.$router.push({ name: Registrasi });
    },
    // methods integrasi get data table
    async getAllQueue() {
      const queues = await this.$store.dispatch("getAllQueues");
      console.log("antrian dari method: ", queues);
      this.queues = queues;
    },
    hitungPage(totalitem) {
      this.totalPage = totalitem;
    },
    closeDelete() {
      this.dialogDelete = false;
      this.$nextTick(() => {
        this.selectedItemIndex = -1;
      });
    },
    deleteItemConfirm() {
      this.$store.dispatch("deleteQueues", this.getid);
      this.queues.splice(this.selectedItemIndex, 1);
      this.closeDelete();
    },
    deleteItem(item, id) {
      this.getid = id;
      this.selectedItemIndex = this.identity.indexOf(item);
      this.dialogDelete = true;
    },
  },
  mounted() {
    this.getAllQueue();
  },
};
</script>

<style>
.JudulHeader {
  font-family: "Lato";
  font-style: normal;
  font-weight: 500;
  font-size: 16px;
  line-height: 19px;
  letter-spacing: 0.0015em;

  /* color 7 */

  color: #ffffff;
}
.MyHeader {
  background-color: #613A03 !important;
}
/* tanya ke mentor */
.aksi {
  border-radius: 10px;
}
</style>
