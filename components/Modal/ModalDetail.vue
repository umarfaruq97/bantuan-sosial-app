<template>
  <b-modal
    v-model="showData"
    scrollable
    :size="'xl'"
    title="Data Detail Penerima Bansos"
    :no-close-on-backdrop="true"
    :no-close-on-esc="true"
    :ok-title="'Tutup'"
    :ok-variant="'info'"
    ok-only
    @hide="$emit('close')"
    @close="$emit('close')"
    @ok="$emit('close')"
  >
    <b-container>
      <b-row>
        <b-col>
          <div class="text-dark">Foto KTP</div>
          <b-img thumbnail fluid :src="data.fotoKtp" alt="Foto Ktp"></b-img>
        </b-col>
        <b-col>
          <div class="text-dark">Foto Kartu Keluarga</div>
          <b-img
            thumbnail
            fluid
            :src="data.fotoKartuKeluarga"
            alt="Foto Kartu Keluarga"
          ></b-img>
        </b-col>
      </b-row>
      <b-row>
        <b-col>
          <div class="text-dark mt-2">Nama Lengkap</div>
          <div class="bg-secondary rounded text-white p-2 mt-2">
            {{ data.nama }}
          </div>
        </b-col>
      </b-row>

      <b-row>
        <b-col>
          <div class="text-dark mt-2">NIK</div>
          <div class="bg-secondary rounded text-white p-2 mt-2">
            {{ data.nik }}
          </div>
        </b-col>
      </b-row>
      <b-row>
        <b-col>
          <div class="text-dark mt-2">Nomor Kartu Keluarga</div>
          <div class="bg-secondary rounded text-white p-2 mt-2">
            {{ data.nomorKartuKeluarga }}
          </div>
        </b-col>
      </b-row>
      <b-row>
        <b-col>
          <div class="text-dark mt-2">Umur</div>
          <div class="bg-secondary rounded text-white p-2 mt-2">
            {{ data.umur }} tahun
          </div>
        </b-col>
      </b-row>
      <b-row>
        <b-col>
          <div class="text-dark mt-2">Jenis Kelamin</div>
          <div class="bg-secondary rounded text-white p-2 mt-2">
            {{ data.jenisKelamin }}
          </div>
        </b-col>
      </b-row>
      <b-row>
        <b-col>
          <div class="text-dark mb-2">Alamat Lengkap</div>
          <div class="bg-secondary rounded text-white p-2 mt-2">
            {{ alamatLengkap }}
          </div>
        </b-col>
      </b-row>
      <b-row>
        <b-col>
          <div class="text-dark mt-2">Penghasilan Sebelum Pandemi</div>
          <div class="bg-secondary rounded text-white p-2 mt-2">
            {{ currencyFormat(data.penghasilanSebelumPandemi) }}
          </div>
        </b-col>
      </b-row>
      <b-row>
        <b-col>
          <div class="text-dark mt-2">Penghasilan Setelah Pandemi</div>
          <div class="bg-secondary rounded text-white p-2 mt-2">
            {{ currencyFormat(data.penghasilanSetelahPandemi) }}
          </div>
        </b-col>
      </b-row>
      <b-row>
        <b-col>
          <div class="text-dark mt-2">Alasan Membutuhkan Bantuan</div>
          <div class="bg-secondary rounded text-white p-2 mt-2">
            {{ data.alasan }}
          </div>
        </b-col>
      </b-row>
    </b-container>
  </b-modal>
</template>

<script>
export default {
  name: 'ModalDetail',
  props: {
    data: {
      type: Object,
      default: () => ({}),
    },
  },
  data() {
    return {
      showData: true,
      fields: [],
    }
  },
  computed: {
    alamatLengkap() {
      const completed = `${this.data.alamat} RT ${this.data.rt} RW ${this.data.rw} Kelurahan/Desa ${this.data.kelurahanDesa.name} Kecamatan ${this.data.kecamatan.name} ${this.data.kabKota.name} ${this.data.provinsi.name}`
      return completed
    },
  },
  methods: {
    currencyFormat(value) {
      return Intl.NumberFormat('id-ID', {
        style: 'currency',
        currency: 'IDR',
      }).format(value)
    },
  },
}
</script>

<style></style>
