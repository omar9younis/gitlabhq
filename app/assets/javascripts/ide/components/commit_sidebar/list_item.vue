<script>
import { mapActions } from 'vuex';
import Icon from '~/vue_shared/components/icon.vue';

export default {
  components: {
    Icon,
  },
  props: {
    file: {
      type: Object,
      required: true,
    },
  },
  computed: {
    iconName() {
      return this.file.tempFile ? 'file-addition' : 'file-modified';
    },
    iconClass() {
      return `multi-file-${this.file.tempFile ? 'addition' : 'modified'} append-right-8`;
    },
  },
  methods: {
    ...mapActions(['discardFileChanges', 'updateViewer', 'openPendingTab']),
    openFileInEditor(file) {
      return this.openPendingTab(file).then(changeViewer => {
        if (changeViewer) {
          this.updateViewer('diff');
        }
      });
    },
  },
};
</script>

<template>
  <div class="multi-file-commit-list-item">
    <button
      type="button"
      class="multi-file-commit-list-path"
      @click="openFileInEditor(file)">
      <span class="multi-file-commit-list-file-path">
        <icon
          :name="iconName"
          :size="16"
          :css-classes="iconClass"
        />{{ file.path }}
      </span>
    </button>
    <button
      type="button"
      class="btn btn-blank multi-file-discard-btn"
      @click="discardFileChanges(file.path)"
    >
      Discard
    </button>
  </div>
</template>
