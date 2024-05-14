<template>
  <div>
    <div>
      <h1 class="text-red-600 mb">Tea Maker App</h1>
    </div>
    <div>
      <form>
        <input
          type="text"
          v-model="teaMaker.name"
          placeholder="Tea maker name"
        />
        <input type="checkbox" v-model="teaMaker.milk" name="milk" id="milk" />
        <label for="milk">Milk</label>
        <input
          type="number"
          v-model="teaMaker.sugars"
          max="5"
          min="0"
          name="sugar"
          id="sugar"
        />
        <label for="sugar">Sugar</label>
      </form>
      <button @click="addTeaMaker">Add a tea maker</button>
    </div>
    <div>
      <h2>{{ teaMaker.name }} has to make the tea</h2>
    </div>
  </div>
</template>

<script setup>
const teaMakers = ref([]);

const teaMaker = reactive({
  name: "",
  milk: false,
  sugars: 0,
  teaRoundsMade: 0
});

const addTeaMaker = (teaMaker) => {
  if (teaMaker.value != "") {
    // create array of object with tea maker name, sugar, milk, and tea rounds made
    const teaMakerObject = {
      id: "",
      name: teaMaker.name,
      sugars: teaMaker.sugars,
      milk: teaMaker.milk,
      teaRoundsMade: 0
    };
  }
  //push to teaMakers array
};

console.log(teaMakers);
</script>

<style lang="scss" scoped></style>
