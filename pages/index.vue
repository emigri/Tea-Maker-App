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
    <div v-for="(teaMaker, index) in teaMakers" :key="index">
      {{ teaMaker.name }}
      {{ teaMaker.milk ? "Milk" : "No milk" }}
      {{ teaMaker.sugars < 1 ? "No " : teaMaker.sugars }} sugar
    </div>
    <div>
      <button @click="clearTeaMakers">Clear</button>
      <button @click="resetRounds">Reset rounds</button>
    </div>
  </div>
</template>

<script setup>
const teaMakers = ref([]);

const teaMaker = reactive({
  name: "",
  milk: false,
  sugars: 0
});

let id = 0;

const addTeaMaker = () => {
  if (teaMaker.value != "") {
    // create an object with tea maker's name, sugar, milk, and tea rounds made
    const teaMakerObject = {
      id: id++,
      name: teaMaker.name,
      sugars: teaMaker.sugars,
      milk: teaMaker.milk,
      teaRoundsMade: 0
    };
    //push to teaMakers array
    teaMakers.value.push(teaMakerObject);

    //reset fields when someone has been added
    teaMaker.name = "";
    teaMaker.sugars = 0;
    teaMaker.milk = false;
  }
};

const clearTeaMakers = () => {
  teaMakers.value.splice(0);
};

const resetRounds = () => {
  teaMakers.value.forEach((teaMaker) => {
    teaMaker.teaRoundsMade = 0;
  });
};
</script>

<style lang="scss" scoped></style>
