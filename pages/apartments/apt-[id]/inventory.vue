<template>
  <div class="flex flex-col justify-center">
    <h3 class="text-3xl text-indigo-600 text-center mt-4 mb-4">
      Inventory Listing for {{ id }}
    </h3>
    <p class="font-bold text-center">Item Name || Item Quantity</p>
    <ul>
      <div v-if="inventory !== null" class="flex justify-center">
        <br />
        <li v-for="(item, index) in inventory" :key="index">
          {{ item.name }} - {{ item.qty }}
        </li>
      </div>
    </ul>
  </div>
  <div class="flex justify-center mt-4">
    <button className="btn btn-accent" @click="approveEdit">Approve</button>
  </div>
</template>

<script>
import { useRoute } from "vue-router";

export default {
  setup() {
    const { id } = useRoute().params;
    const queryString = useRoute().query.inventory;
    const inventory = JSON.parse(queryString);

    const approveEdit = () => {
      try {
        const apartments = JSON.parse(localStorage.getItem("apartments"));
        const index = apartments.findIndex(
          (apartment) => apartment.id === Number(id)
        );
        if (index !== -1) {
          apartments[index].inventory = inventory;
          localStorage.setItem("apartments", JSON.stringify(apartments));
          console.log(apartments);
          alert("Inventory updated successfully");
        }
      } catch (error) {
        console.log(error);
        alert("Inventory failed to update");
      }
    };

    return {
      id,
      inventory,
      approveEdit,
    };
  },
};
</script>
