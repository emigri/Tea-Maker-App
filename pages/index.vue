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
    <button @click="pickTeaMaker">Pick a tea maker!</button>
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
//clear tea maker array
const clearTeaMakers = () => {
  teaMakers.value.splice(0);
};

// reset round for each tea maker
const resetRounds = () => {
  teaMakers.value.forEach((teaMaker) => {
    teaMaker.teaRoundsMade = 0;
  });
};

const pickTeaMaker = () => {
  // create new array of tea makers

  // randomly select a teaMaker to make a round of tea

  // pick teamaker with least rounds made

}
</script>

<style lang="scss" scoped></style>
