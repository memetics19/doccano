<template>
  <div>
    <action-menu
      :items="menuItems"
      @create="createDialog=true"
      @upload="importDialog=true"
      @download="handleDownload"
    />
    <base-dialog :dialog="createDialog">
      <label-creation-form
        :create-label="createLabel"
        :keys="shortkeys"
        @close="createDialog=false"
      />
    </base-dialog>
    <base-dialog :dialog="importDialog">
      <label-import-form
        :upload-label="uploadLabel"
        @close="importDialog=false"
      />
    </base-dialog>
  </div>
</template>

<script>
import { mapActions, mapGetters } from 'vuex'
import ActionMenu from '@/components/molecules/ActionMenu'
import BaseDialog from '@/components/molecules/BaseDialog'
import LabelCreationForm from '@/components/organisms/labels/LabelCreationForm'
import LabelImportForm from '@/components/organisms/labels/LabelImportForm'

export default {
  components: {
    ActionMenu,
    BaseDialog,
    LabelCreationForm,
    LabelImportForm
  },

  data() {
    return {
      createDialog: false,
      importDialog: false,
      menuItems: [
        { title: 'Create a Label', icon: 'mdi-pencil', event: 'create' },
        { title: 'Import Labels', icon: 'mdi-upload', event: 'upload' },
        { title: 'Export Labels', icon: 'mdi-download', event: 'download' }
      ]
    }
  },

  computed: {
    ...mapGetters('labels', ['shortkeys'])
  },

  created() {
    this.setCurrentProject(this.$route.params.id)
  },

  methods: {
    ...mapActions('labels', ['createLabel', 'uploadLabel', 'exportLabels']),
    ...mapActions('projects', ['setCurrentProject']),

    handleDownload() {
      this.exportLabels({
        projectId: this.$route.params.id
      })
    }
  }
}
</script>
