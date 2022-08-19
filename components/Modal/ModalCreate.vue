<template>
  <b-modal
    v-model="showData"
    :no-close-on-backdrop="true"
    scrollable
    :no-close-on-esc="true"
    :ok-title="'Submit'"
    :ok-disabled="!acceptTermsCondition"
    :hide-footer="true"
    @hide="$emit('close')"
    @close="$emit('close')"
  >
    <template #modal-title>
      {{
        mode === 'create'
          ? 'Menambahkan data penerima baru'
          : 'Memperbarui data penerima'
      }}
    </template>
    <b-form-group label="Nama Lengkap" label-for="nama">
      <b-form-input id="nama" v-model="payload.nama"></b-form-input>
    </b-form-group>
    <div
      v-show="validation.nama"
      class="text-danger"
      style="margin-top: -1rem; margin-bottom: 1rem"
    >
      Nama Lengkap wajib diisi
    </div>
    <b-form-group label="NIK" label-for="nik">
      <b-form-input id="nik" v-model="payload.nik" type="number"></b-form-input>
    </b-form-group>
    <div
      v-show="validation.nik"
      class="text-danger"
      style="margin-top: -1rem; margin-bottom: 1rem"
    >
      NIK wajib diisi
    </div>
    <b-form-group label="Nomor Kartu Keluarga" label-for="nomorKartuKeluarga">
      <b-form-input
        id="nomorKartuKeluarga"
        v-model="payload.nomorKartuKeluarga"
        type="number"
      ></b-form-input>
    </b-form-group>
    <div
      v-show="validation.nomorKartuKeluarga"
      class="text-danger"
      style="margin-top: -1rem; margin-bottom: 1rem"
    >
      Nomor Kartu Keluarga wajib diisi
    </div>
    <b-form-group label="Foto KTP" label-for="filenameKtp">
      <b-form-file
        id="filenameKtp"
        :placeholder="
          payload.filenameKtp ? payload.filenameKtp : 'No file chosen'
        "
        accept=".jpg, .png, .jpeg, .jfif"
        @change="handleChangeFoto($event, 'Ktp')"
      ></b-form-file>
    </b-form-group>
    <div
      v-show="validation.filenameKtp"
      class="text-danger"
      style="margin-top: -1rem; margin-bottom: 1rem"
    >
      Foto KTP wajib dilampirkan dan file tidak boleh lebih dari 2MB
    </div>
    <b-form-group label="Foto Kartu Keluarga" label-for="filenameKartuKeluarga">
      <b-form-file
        id="filenameKartuKeluarga"
        :placeholder="
          payload.filenameKartuKeluarga
            ? payload.filenameKartuKeluarga
            : 'No file chosen'
        "
        accept=".jpg, .png, .jpeg, .jfif"
        @change="handleChangeFoto($event, 'KartuKeluarga')"
      ></b-form-file>
    </b-form-group>
    <div
      v-show="validation.filenameKartuKeluarga"
      class="text-danger"
      style="margin-top: -1rem; margin-bottom: 1rem"
    >
      Foto Kartu Keluarga wajib dilampirkan dan file tidak boleh lebih dari 2MB
    </div>
    <b-form-group label="Umur" label-for="umur">
      <b-form-input
        id="umur"
        v-model="payload.umur"
        type="number"
      ></b-form-input>
    </b-form-group>
    <div
      v-show="validation.umur"
      class="text-danger"
      style="margin-top: -1rem; margin-bottom: 1rem"
    >
      Umur wajib diisi dan harus lebih besar atau sama dengan 25 tahun
    </div>
    <b-form-group label="Jenis Kelamin">
      <b-form-radio-group
        id="jenisKelamin"
        v-model="payload.jenisKelamin"
        :options="optionsJenisKelamin"
        name="radio-options"
      ></b-form-radio-group>
    </b-form-group>
    <div
      v-show="validation.jenisKelamin"
      class="text-danger"
      style="margin-top: -1rem; margin-bottom: 1rem"
    >
      Jenis Kelamin wajib diisi
    </div>
    <b-form-group label="Provinsi" label-for="provinsi">
      <b-form-select
        id="provinsi"
        :value="payload.provinsi"
        @input="handleChangeProvinsi($event)"
      >
        <template #first>
          <b-form-select-option :value="null" disabled>
            -- Silahkan Pilih Provinsi --
          </b-form-select-option>
        </template>

        <b-form-select-option
          v-for="(option, index) in options.provinsi"
          :key="`opsi-provinsi-${index}`"
          :value="option"
          selected
        >
          {{ option.name }}
        </b-form-select-option>
      </b-form-select>
    </b-form-group>
    <div
      v-show="validation.provinsi"
      class="text-danger"
      style="margin-top: -1rem; margin-bottom: 1rem"
    >
      Provinsi wajib dipilih
    </div>
    <b-form-group label="Kabupaten/Kota" label-for="kabKota">
      <b-form-select
        id="kabKota"
        :value="payload.kabKota"
        :disabled="!payload.provinsi"
        @input="handleChangeKabKota($event)"
      >
        <template #first>
          <b-form-select-option :value="null" disabled>
            -- Silahkan Pilih Kabupaten Kota --
          </b-form-select-option>
        </template>

        <b-form-select-option
          v-for="(option, index) in options.kabKota"
          :key="`opsi-kabKota-${index}`"
          :value="option"
        >
          {{ option.name }}
        </b-form-select-option>
      </b-form-select>
    </b-form-group>
    <div
      v-show="validation.kabKota"
      class="text-danger"
      style="margin-top: -1rem; margin-bottom: 1rem"
    >
      Kabupaten/Kota wajib dipilih
    </div>
    <b-form-group label="Kecamatan" label-for="kecamatan">
      <b-form-select
        id="kecamatan"
        :value="payload.kecamatan"
        :disabled="!payload.kabKota"
        @input="handleChangeKecamatan($event)"
      >
        <template #first>
          <b-form-select-option :value="null" disabled>
            -- Silahkan Pilih Kecamatan --
          </b-form-select-option>
        </template>

        <b-form-select-option
          v-for="(option, index) in options.kecamatan"
          :key="`opsi-kecamatan-${index}`"
          :value="option"
        >
          {{ option.name }}
        </b-form-select-option>
      </b-form-select>
    </b-form-group>
    <div
      v-show="validation.kecamatan"
      class="text-danger"
      style="margin-top: -1rem; margin-bottom: 1rem"
    >
      Kecamatan wajib dipilih
    </div>
    <b-form-group label="Kelurahan/Desa" label-for="kelurahanDesa">
      <b-form-select
        id="kelurahanDesa"
        :value="payload.kelurahanDesa"
        :disabled="!payload.kecamatan"
        @input="handleChangeKelurahan($event)"
      >
        <template #first>
          <b-form-select-option :value="null" disabled>
            -- Silahkan Pilih Kelurahan/Desa --
          </b-form-select-option>
        </template>

        <b-form-select-option
          v-for="(option, index) in options.kelurahanDesa"
          :key="`opsi-kelurahanDesa-${index}`"
          :value="option"
        >
          {{ option.name }}
        </b-form-select-option>
      </b-form-select>
    </b-form-group>
    <div
      v-show="validation.kelurahanDesa"
      class="text-danger"
      style="margin-top: -1rem; margin-bottom: 1rem"
    >
      Kelurahan/Desa wajib dipilih
    </div>
    <b-form-group label="Alamat Lengkap" label-for="alamat">
      <b-form-textarea
        id="alamat"
        v-model="payload.alamat"
        placeholder="Isikan alamat lengkap"
        rows="3"
        max-rows="7"
        maxlength="255"
      ></b-form-textarea>
    </b-form-group>
    <div
      v-show="validation.alamat"
      class="text-danger"
      style="margin-top: -1rem; margin-bottom: 1rem"
    >
      Alamat wajib diisi
    </div>
    <b-form-group label="RT" label-for="rt">
      <b-form-input
        id="rt"
        v-model="payload.rt"
        :placeholder="'Contoh : 001'"
      ></b-form-input>
    </b-form-group>
    <div
      v-show="validation.rt"
      class="text-danger"
      style="margin-top: -1rem; margin-bottom: 1rem"
    >
      Nomor RT wajib diisi
    </div>
    <b-form-group label="RW" label-for="rw">
      <b-form-input
        id="rw"
        v-model="payload.rw"
        :placeholder="'Contoh : 004'"
      ></b-form-input>
    </b-form-group>
    <div
      v-show="validation.rw"
      class="text-danger"
      style="margin-top: -1rem; margin-bottom: 1rem"
    >
      Nomor RW wajib diisi
    </div>

    <b-form-group
      label="Penghasilan Sebelum Pandemi"
      label-for="penghasilanSebelumPandemi"
    >
      <b-form-input
        id="penghasilanSebelumPandemi"
        v-model="payload.penghasilanSebelumPandemi"
        :type="'number'"
      ></b-form-input>
    </b-form-group>
    <div
      v-show="validation.penghasilanSebelumPandemi"
      class="text-danger"
      style="margin-top: -1rem; margin-bottom: 1rem"
    >
      Penghasilan Sebelum Pandemi wajib diisi
    </div>

    <b-form-group
      label="Penghasilan Setelah Pandemi"
      label-for="penghasilanSetelahPandemi"
    >
      <b-form-input
        id="penghasilanSetelahPandemi"
        v-model="payload.penghasilanSetelahPandemi"
        :type="'number'"
      ></b-form-input>
    </b-form-group>
    <div
      v-show="validation.penghasilanSetelahPandemi"
      class="text-danger"
      style="margin-top: -1rem; margin-bottom: 1rem"
    >
      Penghasilan Setelah Pandemi wajib diisi
    </div>

    <b-form-group label="Alasan Membutuhkan Bantuan" label-for="alasan">
      <b-form-select
        id="alasan"
        v-model="payload.alasan"
        :options="optionsAlasan"
      >
        <template #first>
          <b-form-select-option :value="null" disabled>
            -- Silahkan Pilih Alasan Membutuhkan Bantuan --
          </b-form-select-option>
        </template>
      </b-form-select>
    </b-form-group>
    <div
      v-show="validation.alasan"
      class="text-danger"
      style="margin-top: -1rem; margin-bottom: 1rem"
    >
      Alasan membutuhkan bantuan wajib diisi
    </div>

    <b-form-group
      v-show="payload.alasan === 'Lainnya'"
      label="Alasan Lainnya"
      label-for="alasanLain"
    >
      <b-form-input
        id="alasanLain"
        v-model="payload.alasanLain"
        :placeholder="'Contoh : Untuk biaya berobat'"
      ></b-form-input>
    </b-form-group>
    <div
      v-show="validation.alasanLain"
      class="text-danger"
      style="margin-top: -1rem; margin-bottom: 1rem"
    >
      Silahkan disi terlebih dahulu alasan lainnya
    </div>
    <div
      class="d-flex align-items-center p-2 rounded"
      :style="'background-color: aliceblue;'"
    >
      <b-form-checkbox
        id="acceptTermsCondition"
        v-model="acceptTermsCondition"
        name="acceptTermsCondition"
        :value="true"
        :unchecked-value="false"
      >
        Saya menyatakan bahwa data yang diisikan adalah benar dan siap
        mempertanggungjawabkan apabila ditemukan ketidaksesuaian dalam data
        tersebut.
      </b-form-checkbox>
    </div>
    <div class="d-flex justify-content-end mt-3">
      <b-button
        variant="primary"
        :disabled="!acceptTermsCondition"
        @click="handleSubmit()"
      >
        Submit
      </b-button>
    </div>
  </b-modal>
</template>

<script>
import axios from 'axios'
import { optionsReason } from '~/static/data'
export default {
  name: 'ModalCreate',
  props: {
    mode: {
      type: String,
      default: 'create',
    },
    data: {
      type: Object,
      default: () => ({}),
    },
  },
  data() {
    return {
      showData: true,
      payload: this.data,
      validation: {
        nama: false,
        nik: false,
        nomorKartuKeluarga: false,
        filenameKtp: false,
        filenameKartuKeluarga: false,
        umur: false,
        jenisKelamin: false,
        provinsi: false,
        kabKota: false,
        kecamatan: false,
        kelurahanDesa: false,
        alamat: false,
        rt: false,
        rw: false,
        penghasilanSebelumPandemi: false,
        penghasilanSetelahPandemi: false,
        alasan: false,
        alasanLain: false,
      },
      optionsJenisKelamin: [
        { value: 'Laki-laki', text: 'Laki-laki' },
        { value: 'Perempuan', text: 'Perempuan' },
      ],
      options: {
        provinsi: [],
        kabKota: [],
        kecamatan: [],
        kelurahanDesa: [],
      },
      optionsAlasan: optionsReason,
      acceptTermsCondition: false,
      baseUrl: 'https://www.emsifa.com/api-wilayah-indonesia/api',
    }
  },
  watch: {
    data(newValue) {
      this.payload = newValue
    },
  },
  async created() {
    if (this.data.alasan === '') {
      this.showInputAlasan = true
    }
    await this.fetchProvinsi()
    if (this.mode === 'update') {
      await this.fetchKabKota()
      await this.fetchKecamatan()
      await this.fetchKelurahan()
      await this.fetchProvinsi()
    }
  },
  methods: {
    async fetchProvinsi() {
      const result = await axios
        .get(`${this.baseUrl}/provinces.json`)
        .then((res) => res.data)
        .catch((err) => err)
      if (result.length > 0) {
        this.options.provinsi = result
      } else {
        this.options.provinsi = []
      }
    },
    async handleChangeProvinsi(data) {
      if (data.id && data.name) {
        this.payload.provinsi = data
        this.payload.kabKota = null
        this.payload.kecamatan = null
        this.payload.kelurahanDesa = null
        this.options.kabKota = []
        this.options.kecamatan = []
        this.options.kelurahanDesa = []
        await this.fetchKabKota()
      }
    },
    async fetchKabKota() {
      const result = await axios
        .get(`${this.baseUrl}/regencies/${this.payload.provinsi.id}.json`)
        .then((res) => res.data)
        .catch((err) => err)
      if (result.length > 0) {
        this.options.kabKota = result
      } else {
        this.options.kabKota = []
      }
    },
    async handleChangeKabKota(data) {
      if (data && data.id) {
        this.payload.kabKota = data
        this.payload.kecamatan = null
        this.payload.kelurahanDesa = null
        this.options.kecamatan = []
        this.options.kelurahanDesa = []
        await this.fetchKecamatan()
      }
    },
    async fetchKecamatan() {
      const result = await axios
        .get(`${this.baseUrl}/districts/${this.payload.kabKota.id}.json`)
        .then((res) => res.data)
        .catch((err) => err)
      if (result.length > 0) {
        this.options.kecamatan = result
      } else {
        this.options.kecamatan = []
      }
    },
    async handleChangeKecamatan(data) {
      if (data && data.id) {
        this.payload.kecamatan = data
        this.payload.kelurahanDesa = null
        this.options.kelurahanDesa = []
        await this.fetchKelurahan()
      }
    },
    async fetchKelurahan() {
      const result = await axios
        .get(`${this.baseUrl}/villages/${this.payload.kecamatan.id}.json`)
        .then((res) => res.data)
        .catch((err) => err)
      if (result.length > 0) {
        this.options.kelurahanDesa = result
      } else {
        this.options.kelurahanDesa = []
      }
    },
    handleChangeKelurahan(data) {
      if (data && data.id) {
        this.payload.kelurahanDesa = data
      }
    },
    handleChangeFoto(e, field) {
      this.validation[`filename` + field] = false
      this.payload[`filename` + field] = null
      if (typeof e.target.files[0] !== 'undefined') {
        if (e.target.files[0].size <= 2000000) {
          this.payload[`filename` + field] = e.target.files[0].name
          const reader = new FileReader()
          reader.readAsDataURL(e.target.files[0])
          reader.onload = (e) => {
            this.payload[`foto` + field] = e.target.result
          }
        } else {
          this.payload[`filename` + field] = null
          this.payload[`foto` + field] = null
          this.validation[`filename` + field] = true
        }
      } else {
        this.payload[`filename` + field] = null
        this.payload[`foto` + field] = null
      }
    },
    handleSubmit() {
      this.resetValidation()
      for (const key of Object.keys(this.validation)) {
        if (!this.payload[key]) {
          if (key === 'alasanLain' && this.payload.alasan) {
            continue
          } else {
            this.validation[key] = true
            document.getElementById(key).scrollIntoView({
              block: 'center',
              inline: 'nearest',
              behavior: 'smooth',
            })
            return
          }
        }
        if (key === 'umur' && this.payload.umur < 25) {
          this.validation.umur = true
          document.getElementById(key).scrollIntoView({
            block: 'center',
            inline: 'nearest',
            behavior: 'smooth',
          })
          return
        }
      }
      this.$emit('handle-submit', this.payload)
    },
    resetValidation() {
      for (const key of Object.keys(this.validation)) {
        this.validation[key] = false
      }
    },
  },
}
</script>

<style></style>
