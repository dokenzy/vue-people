<template>
  <v-toolbar
    class="tag-filter"
    floating
    light
  >
    <v-btn
      icon
      light
      @click="openMenu">
      <v-icon>filter_list</v-icon>
    </v-btn>
    <v-menu
      v-model="dropdownOpen"
      :close-on-content-click="false"
      activator=".tag-filter"
      light
    >
      <v-select
        ref="tagSelect"
        :items="tagList"
        v-model="selectedTags"
        label="Select"
        prepend-icon="search"
        light
        solo
        single-line
        clearable
        autocomplete
        multiple
      />
    </v-menu>
    <div class="chips">
      <v-chip
        v-for="tag in selectedTags"
        :key="tag"
        close
        @input="removeTag(tag)"
      >
        {{ tag }}
      </v-chip>
    </div>


  </v-toolbar>
</template>

<script>
import {mapGetters, mapActions} from 'vuex';

export default {
  name: 'TagFilter',
  data () {
    return {
      dropdownOpen: false
    };
  },
  computed: {
    ...mapGetters({
      tagList: 'getTags',
      getSelectedTags: 'people/getSelectedTags'
    }),
    selectedTags: {
      get() {
        return this.getSelectedTags;
      },
      set(selected) {
        this.setSelectedTags(selected);
      }
    },
    showTagsChips() {
      return !this.dropdownOpen && this.selectedTags.length > 0;
    }
  },
  methods: {
    ...mapActions({
      setSelectedTags: 'people/setSelectedTags'
    }),
    openMenu() {
      this.dropdownOpen = !this.dropdownOpen;
      this.$nextTick(() => {
        window.setTimeout(() => {
          this.$refs.tagSelect.focus();
        }, 100);
      });
    },
    removeTag(tag) {
      this.setSelectedTags(this.selectedTags.filter(t => t !== tag));
    }
  }
};
</script>

<style lang="less">
  .tag-filter {
    position: absolute;
    top: 16px;
    left: 0;
    z-index: 10;
  }
</style>