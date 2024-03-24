<template>
  <div class="fighters-page">
    <div class="card">
      <DataTable
        :value="fighters"
        :paginator="true"
        :rows="4"
        dataKey="id"
        :filters.sync="filters"
        filterDisplay="menu"
        :loading="loading"
        responsiveLayout="scroll"
        :globalFilterFields="['name', 'belt', 'weight', 'birth', 'country']"
      >
        <template #header>
          <div class="flex justify-content-between">
            <Button type="button" icon="pi pi-filter-slash" label="Clear" class="p-button-outlined" @click="clearFilter" />
            <span class="p-input-icon-left">
              <i class="pi pi-search" />
              <InputText v-model="filters['global'].value" placeholder="Recherche par mot-clé" />
            </span>
          </div>
        </template>
        <template #empty>
          Aucun combattant trouvé.
        </template>
        <template #loading>
          Chargement des combattants. Veuillez patienter.
        </template>
        <Column field="name" header="Nom" :styles="{'min-width':'12rem'}" sortable>
          <template #body="{data}">
            {{ data.name }}
          </template>
          <template #filter="{filterModel}">
            <InputText type="text" v-model="filterModel.value" class="p-column-filter" placeholder="Rechercher par nom" />
          </template>
        </Column>
        <Column field="belt" header="Ceinture" :styles="{'min-width':'12rem'}" sortable>
          <template #body="{data}">
            {{ data.belt }}
          </template>
          <template #filter="{filterModel}">
            <InputText type="text" v-model="filterModel.value" class="p-column-filter" placeholder="Rechercher par ceinture" />
          </template>
        </Column>
        <Column field="weight" header="Poids" :styles="{'min-width':'12rem'}" sortable>
          <template #body="{data}">
            {{ data.weight }}
          </template>
          <template #filter="{filterModel}">
            <InputText type="text" v-model="filterModel.value" class="p-column-filter" placeholder="Rechercher par poids" />
          </template>
        </Column>
        <Column field="birth" header="Date de naissance" :styles="{'min-width':'12rem'}" sortable>
          <template #body="{data}">
            {{ data.birth }}
          </template>
          <template #filter="{filterModel}">
            <InputText type="text" v-model="filterModel.value" class="p-column-filter" placeholder="Rechercher par date de naissance" />
          </template>
        </Column>
        <Column field="country" header="Pays" :styles="{'min-width':'12rem'}" sortable>
          <template #body="{data}">
            {{ data.country }}
          </template>
          <template #filter="{filterModel}">
            <InputText type="text" v-model="filterModel.value" class="p-column-filter" placeholder="Rechercher par pays" />
          </template>
        </Column>
        <Column header="Profil" :styles="{'min-width':'8rem'}">
          <template #body="{data}">
            <router-link :to="`/fighters/${data.slug}`" class="profile">Voir le profil</router-link>
          </template>
        </Column>
      </DataTable>
    </div>
  </div>
</template>

<script>
import { ref } from 'vue';
import { FilterMatchMode } from 'primevue/api';
import DataTable from 'primevue/datatable';
import Column from 'primevue/column';
import InputText from 'primevue/inputtext';
import Button from 'primevue/button';

export default {
  components: {
    DataTable,
    Column,
    Button,
    InputText
  },
  data() {
    return {
      filters: {
        global: { value: null, matchMode: FilterMatchMode.CONTAINS },
        weight: { value: null, matchMode: FilterMatchMode.STARTS_WITH },
        birth: { value: null, matchMode: FilterMatchMode.STARTS_WITH },
        country: { value: null, matchMode: FilterMatchMode.STARTS_WITH }
      },
      fighters: []
    };
  },
  methods: {
    clearFilter() {
      this.filters = {
        global: { value: null, matchMode: FilterMatchMode.CONTAINS },
        weight: { value: null, matchMode: FilterMatchMode.STARTS_WITH },
        birth: { value: null, matchMode: FilterMatchMode.STARTS_WITH },
        country: { value: null, matchMode: FilterMatchMode.STARTS_WITH }
      };
    }
  },
  async mounted() {
    try {
      const response = await this.$axios.get('/api/fighters');
      this.fighters = response.data.data || [];
    } catch (error) {
      console.error('Erreur lors de la récupération des combattants:', error);
      this.fighters = [];
    }
  }
};
</script>

<style scoped>
/* Ajoutez vos styles CSS ici */
</style>
