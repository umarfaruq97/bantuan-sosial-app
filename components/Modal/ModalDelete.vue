<template>
  <b-modal
    v-model="showData"
    :size="'sm'"
    title="Menghapus data penerima bansos"
    :no-close-on-backdrop="true"
    :no-close-on-esc="true"
    :ok-title="'Delete'"
    :ok-variant="'danger'"
    @hide="$emit('close')"
    @close="$emit('close')"
    @ok="$emit('handle-submit-delete')"
  >
    Apakah anda yakin hendak menghapus penerima bansos atas nama
    <strong>{{ data.nama }}</strong> ?
  </b-modal>
</template>

<script>
export default {
  name: 'ModalDelete',
  props: {
    data: {
      type: Object,
      default: () => ({}),
    },
  },
  data() {
    return {
      showData: true,
    }
  },
}
</script>

<style></style>
