<template>
  <div class="container">
    <div class="row">
      <ContactsTable>
        <template v-slot:header>
          <h1 class="text-center mb-4">IronContacts</h1>
        </template>
      </ContactsTable>
    </div>
  </div>
</template>

<script setup>
import ContactsTable from "./components/ContactsTable.vue";
</script>

<style>
.container {
  padding: 2rem 0rem;
  font-size: 1.2em;
}

h1 {
  margin: 2rem 0rem 1rem;
}
</style>
