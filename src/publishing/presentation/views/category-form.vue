<script setup>
import {onMounted, ref, toRef, toRefs} from "vue";
import usePublishingStore  from "../../application/publishing.store.js";
import {useI18n} from "vue-i18n";
import {Category} from "../../domain/model/category.entity.js"
import {useRoute, useRouter} from "vue-router";
const router = useRouter();
const route = useRoute();
const store = usePublishingStore();
const {errors, categories} = toRef(store);
const {updateCategory, addCategory} = store;

const {t} = useI18n();
const isEdit = ref(false);
const form = ref({name: ''});
function saveCategory()
{
  const category = new Category({
    id: isEdit.value ? route.params.id : null,
    name: form.value.name,
  });
  if (isEdit.value) updateCategory(category); else addCategory(category);
  navigateBack();
}

function navigateBack()
{
  router.push({name: 'publishing-categories'});
}
onMounted( () => {
  if (route.params.id) {
    console.log("se edita la pagina");
    isEdit.value = true;
    const category = store.getCategoryById(route.params.id);
    if (category) form.value.name = category.name; else navigateBack();
  }
})
</script>

<template>
<div>
  <h1>{{ isEdit.value ? t('category.edit-title') :  t('category.new-title') }}
  </h1>

  <form @submit.prevent="saveCategory">
    <div class="field mb-3">
      <label for="name">{{t('category.name')}}</label>
      <pv-input-text id="name" v-model="form.name" class="w-full" required/>
        <pv-button :label="t('category.save')" icon = "pi pi-save" type="submit"/>
        <pv-button :label="t('category.cancel')" icon = "pi pi-times" class="ml-2" @click="navigateBack"/>
      </div>
  </form>
</div>
</template>

<style scoped>

</style>