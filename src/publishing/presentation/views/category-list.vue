<script setup>
import {computed, onMounted, toRefs} from "vue";
import usePublishingStore from "../../application/publishing.store.js";
const store = usePublishingStore();
const confirm = useConfirm();
import {useConfirm} from "primevue";
const categories = computed(() => store.categories);
const {  categoriesLoaded} = toRefs(store);
const {fetchCategories, deleteCategory} = store;
import {useI18n} from "vue-i18n";
import {useRouter} from "vue-router";
const router = useRouter();
const {t} = useI18n();

onMounted(() => {
  if (!store.categoriesLoaded) {
    fetchCategories();
    categoriesLoaded.value = store.categoriesLoaded;
    console.log(store.categoriesLoaded);
    console.log(categories);
  }
});
function navigateToEditCategory(id) {
  console.log("Edit category clicked");
  router.push({ name: 'publishing-category-edit', params: { id } });
}
const  confirmDeleteCategory= (category) => {
    confirm.require({
      message: t('categories.confirm-delete', {name: category.name}),
      header: t('categories.delete-header'),
      icon: 'pi pi-exclamation-triangle',
      accept: () => {
        deleteCategory(category);
      },
    });
  };

function navigateToNew(){
  router.push({ name: 'publishing-category-new'});
  console.log("New category clicked");
}
</script>

<template>
<div>
  <pv-confirm-dialog  />
  <pv-button  :label="t('categories.new')" class="mb-3" icon="pi pi-plus" @click="navigateToNew"/>
<pv-data-table :loading="!categoriesLoaded"
               :rows="5"
               :rows-per-page-options="[5,10,20]"
               :value="categories"
                paginator
                :rows-per-page="5"
               striped-rows
               table-style="min-width: 50rem">
  <pv-column :header="t('categories.id')"  field="id"  sortable/>
  <pv-column :header="t('categories.name')" field="name" sortable/>

  <pv-column :header="t('categories.actions')">
    <template #body="slotProps">
      <pv-button icon="pi pi-pencil" rounded text @click="navigateToEditCategory(slotProps.data.id)"/>
      <pv-button icon="pi pi-trash" rounded severity="danger" text @click="confirmDeleteCategory(slotProps.data)"/>
    </template>
  </pv-column>
</pv-data-table>
</div>
</template>

<style scoped>

</style>