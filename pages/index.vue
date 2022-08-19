<template>
  <b-container fluid>
    <b-row class="my-2 mr-2" align-h="end">
      <b-button variant="primary" @click="handleCreate()">
        Tambah Penerima Bantuan
      </b-button>
    </b-row>
    <b-row>
      <b-col>
        <b-table
          id="table-bansos"
          striped
          hover
          :items="itemsData"
          :fields="fields"
          responsive
        >
          <template #cell(provinsi)="data">
            {{ data.item.provinsi.name }}
          </template>
          <template #cell(kabKota)="data">
            {{ data.item.kabKota.name }}
          </template>
          <template #cell(kecamatan)="data">
            {{ data.item.kecamatan.name }}
          </template>
          <template #cell(kelurahanDesa)="data">
            {{ data.item.kelurahanDesa.name }}
          </template>
          <template #cell(actions)="data">
            <b-button
              variant="outline-primary"
              @click="handleUpdate(data.item)"
            >
              Edit
            </b-button>
            <b-button variant="outline-info" @click="handleRead(data.item)">
              Detail
            </b-button>
            <b-button variant="outline-danger" @click="handleDelete(data.item)">
              Delete
            </b-button>
          </template>
        </b-table>
      </b-col>
    </b-row>
    <ModalCreate
      v-if="modal.create"
      :data="payload"
      :mode="mode"
      @close="modal.create = false"
      @handle-submit="handleSubmit($event)"
    />

    <ModalDelete
      v-if="modal.delete"
      :data="selectedData"
      @close="modal.delete = false"
      @handle-submit-delete="handleSubmitDelete()"
    />
    <ModalDetail
      v-if="modal.detail"
      :data="selectedData"
      @close="modal.detail = false"
    />
    <b-overlay :show="isLoading" no-wrap></b-overlay>
  </b-container>
</template>

<script>
import { listPenerimaBantuan, emptyPayload } from '~/static/data'
import ModalCreate from '~/components/Modal/ModalCreate.vue'
import ModalDelete from '~/components/Modal/ModalDelete.vue'
import ModalDetail from '~/components/Modal/ModalDetail.vue'
export default {
  name: 'IndexPage',
  components: {
    ModalCreate,
    ModalDelete,
    ModalDetail,
  },
  data() {
    return {
      items: [...listPenerimaBantuan],
      fields: [
        'nama',
        'jenisKelamin',
        'umur',
        'provinsi',
        'kabKota',
        'kecamatan',
        'kelurahanDesa',
        { key: 'actions', label: '' },
      ],
      modal: {
        create: false,
        delete: false,
        detail: false,
      },
      mode: 'create',
      payload: emptyPayload,
      isLoading: false,
      selectedData: {},
      itemsData: [...listPenerimaBantuan],
    }
  },
  watch: {
    isLoading() {
      this.itemsData = [...this.items] // untuk trigger changes
    },
  },
  methods: {
    handleCreate() {
      this.mode = 'create'
      this.modal.create = true
      this.payload = emptyPayload
    },
    handleUpdate(data) {
      this.payload = { ...data }
      this.mode = 'update'
      this.modal.create = true
    },
    handleRead(data) {
      this.selectedData = data
      this.isLoading = true
      setTimeout(() => {
        this.isLoading = false
        this.modal.detail = true
      }, 1500)
    },
    handleDelete(data) {
      this.selectedData = data
      this.modal.delete = true
    },
    handleSubmitDelete() {
      this.modal.delete = false
      this.isLoading = true
      setTimeout(() => {
        const index = this.items.findIndex(
          (el) => el.id === this.selectedData.id
        )
        if (index !== -1) {
          this.items.splice(index, 1)
        }
        this.isLoading = false
      }, 1500)
    },
    handleSubmit(data) {
      this.modal.create = false
      this.isLoading = true
      if (this.mode === 'create') {
        const id = this.items.length + 1
        data.id = id
        setTimeout(() => {
          this.items.push(data)
          this.isLoading = false
        }, 1500)
      } else if (this.mode === 'update') {
        setTimeout(() => {
          const index = this.items.findIndex((el) => el.id === data.id)
          if (index !== -1) {
            this.items[index] = data
          }
          this.isLoading = false
        }, 1500)
      }
    },
  },
}
</script>
