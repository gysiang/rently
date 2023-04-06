<template>
  <h2 class="text-5xl text-indigo-600 text-center mt-4 mb-4">Rently</h2>
  <div
    v-if="apartments !== null"
    class="grid grid-cols-3 gap-2 justify-center w-full ml-10"
  >
    <div v-for="(apartment, index) in apartments" :key="index">
      <div class="card card-compact w-96 bg-base-100 shadow-xl mb-4">
        <figure>
          <img src="/condo.jpeg" alt="apartment" />
        </figure>
        <h1>Address:{{ apartment.address }}</h1>
        <p>Unit No:{{ apartment.floor }}-{{ apartment.door }}</p>

        <NuxtLink
          :to="`/apartments/${apartment.id}`"
          :params="{
            id: apartment.id,
            address: apartment.address,
            floor: apartment.floor,
            door: apartment.door,
          }"
        >
          <button className="btn btn-primary">View Details</button>
        </NuxtLink>
      </div>
    </div>
  </div>
  <p v-else>Loading...</p>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      apartments: null,
    };
  },

  created() {
    axios
      .get("/apartments.json")
      .then((response) => {
        this.apartments = response.data;
        localStorage.setItem("apartments", JSON.stringify(this.apartments));
      })
      .catch((error) => {
        console.error(error);
      });
  },
};
</script>
