<template>
  <div class="flex flex-col justify-center">
    <h2 class="text-5xl text-indigo-600 text-center mt-4 mb-4">Rently</h2>
    <h3 class="text-center mb-4">Apartment Details</h3>
    <div class="card card-compact w-96 bg-base-100 shadow-xl mb-4 mx-auto">
      <figure>
        <img src="/condo.jpeg" alt="apartment" class="mx-auto" />
      </figure>
      <div v-if="apartment && Object.keys(apartment).length > 0" class="m-4">
        <h1>Address:{{ apartment.address }}</h1>
        <p>Unit No:{{ apartment.floor }}-{{ apartment.door }}</p>
      </div>
    </div>
    <div class="flex justify-center items-center">
      <div class="w-1/4">
        <h3 class="text-center mb-4">Inventory List</h3>
        <div v-if="inventory && inventory.length > 0" class="mt-4">
          <ul class="list-disc pl-8">
            <li v-for="(item, index) in inventory" :key="index">
              {{ item.name }} - {{ item.qty }}
            </li>
          </ul>
          <NuxtLink
            :to="{
              path: `/apartments/apt-${apartment.id}/inventory`,
              query: { inventory: JSON.stringify(inventory) },
            }"
          >
            <div class="form-control text-center">
              <button className="btn btn-outline btn-info mt-4">Preview</button>
            </div>
          </NuxtLink>
        </div>
        <br />
        <form @submit.prevent="addInventory">
          <div class="form-control">
            <label class="label" for="item-name">Item Name</label>
            <input
              class="appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
              type="text"
              id="item-name"
              name="item-name"
              required
              v-model="itemName"
            />
          </div>
          <div class="form-control">
            <label class="label" for="item-qty">Item Quantity</label>
            <input
              class="appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
              type="number"
              id="item-qty"
              name="item-qty"
              required
              min="1"
              v-model.number="itemQty"
            />
          </div>
          <div class="form-control text-center">
            <button class="btn btn-primary mt-4" type="submit">Add Item</button>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>

<script setup>
const { id } = useRoute().params;
const getApartment = async () => {
  try {
    const apartments = JSON.parse(localStorage.getItem("apartments"));
    const apartment = apartments.find(
      (apartment) => apartment.id === parseInt(id)
    );
    const inventory = apartment.inventory;
    console.log(apartment);
    console.log(inventory);
    return { apartment, inventory };
  } catch (error) {
    console.error(error);
  }
};

const apartment = ref({});
const inventory = ref([]);
const itemName = ref("");
const itemQty = ref(0);

onMounted(async () => {
  const { apartment: apt, inventory: inv } = await getApartment();
  apartment.value = apt;
  inventory.value = inv;
});

const addInventory = () => {
  if (inventory.value.length < 20) {
    inventory.value.push({ name: itemName.value, qty: itemQty.value });
    itemName.value = "";
    itemQty.value = 0;
  } else {
    alert("You have reached the maximum number of items allowed.");
  }
};
</script>
