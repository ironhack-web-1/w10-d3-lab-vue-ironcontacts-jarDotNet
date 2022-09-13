<template>
  <div class="col-6">
    <slot name="header"></slot>

    <AlertInfo
      :numItems="state.arr.length"
      :totalItems="state.numberOfContacts"
    />

    <ButtonBar
      :sortByPopularityClass="state.sortByPopularityClass"
      :sortByNameClass="state.sortByNameClass"
      @onRandomContacClicked="addRandomContact"
      @onSortByPopulatityClicked="sortByPopulatity"
      @onSortByNameClicked="sortByName"
    />

    <table class="table table-striped table-hover mt-2">
      <thead class="table-success align-middle text-center">
        <tr>
          <th scope="col">Picture</th>
          <th scope="col">Name</th>
          <th scope="col">Popularity</th>
          <th scope="col">Won Oscar</th>
          <th scope="col">Won Emy</th>
          <th scope="col">Actions</th>
        </tr>
      </thead>
      <tbody class="align-middle">
        <tr v-for="contact in state.arr" :key="contact.id">
          <td class="col-md-2">
            <img
              :src="contact.pictureUrl"
              :alt="contact.name"
              class="img-fluid img-thumbnail"
            />
          </td>
          <td>{{ contact.name }}</td>
          <td>{{ contact.popularity.toFixed([2]) }}</td>
          <td><span v-show="contact.wonOscar">🏆</span></td>
          <td><span v-show="contact.wonEmmy">🌟</span></td>
          <td>
            <button
              type="button"
              class="btn btn-danger text-nowrap"
              @click="deleteContact($event)"
              :value="contact.id"
            >
              <i class="bi bi-trash text-light"></i> Delete
            </button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import { reactive, ref, computed, onMounted } from "vue";
import AlertInfo from "./AlertInfo.vue";
import ButtonBar from "./ButtonBar.vue";
import contacts from "../contacts.json";

export default {
  name: "ContactsTable",
  components: {
    AlertInfo,
    ButtonBar,
  },
  setup() {
    let sortByPopularityAscending = false;
    let sortByNameAscending = false;

    const getSortPriorityClass = (ascending) => {
      return ascending ? "bi bi-sort-numeric-down" : "bi bi-sort-numeric-up";
    };

    const getSortNameClass = (ascending) => {
      return ascending ? "bi bi-sort-alpha-down" : "bi bi-sort-alpha-up";
    };

    const state = reactive({
      arr: contacts.slice(0, 5),
      numberOfContacts: contacts.length,
      sortByPopularityClass: getSortPriorityClass(sortByPopularityAscending),
      sortByNameClass: getSortNameClass(sortByNameAscending),
    });

    const excludedContacts = computed(() => {
      return (
        state.arr.map((item) =>
          contacts.findIndex((contact) => contact.id === item.id)
        ) || []
      );
    });

    const addRandomContact = () => {
      const max = contacts.length;
      const nums = [];
      const excluded = [...excludedContacts.value];
      for (let i = 0; i < max; i++) {
        if (!excluded.includes(i)) nums.push(i);
      }
      if (nums.length === 0) return;

      const randomIndex = Math.floor(Math.random() * nums.length);
      const num = nums[randomIndex];
      state.arr.push(contacts[num]);
    };

    const deleteContact = (e) => {
      const contactId = e.target.value;
      state.arr = state.arr.filter((contact) => contact.id !== contactId);
    };

    const sortByPopulatity = () => {
      if (sortByPopularityAscending) {
        state.arr.sort((a, b) => a.popularity - b.popularity);
      } else {
        state.arr.sort((a, b) => b.popularity - a.popularity);
      }
      sortByPopularityAscending = !sortByPopularityAscending;
      state.sortByPopularityClass = getSortPriorityClass(
        sortByPopularityAscending
      );
    };

    const sortByName = () => {
      if (sortByNameAscending) {
        state.arr.sort((a, b) => b.name.localeCompare(a.name));
      } else {
        state.arr.sort((a, b) => a.name.localeCompare(b.name));
      }
      sortByNameAscending = !sortByNameAscending;
      state.sortByNameClass = getSortNameClass(sortByNameAscending);
    };

    return {
      state,
      deleteContact,
      addRandomContact,
      sortByPopulatity,
      sortByName,
    };
  },
};
</script>

<style scoped>
</style>