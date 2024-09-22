<template>
  <div class="h-screen w-screen flex flex-col justify-center items-center">
    <div class="rounded-lg bg-emerald-300 w-1/2 text-center">
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
          <input
            type="checkbox"
            v-model="teaMaker.milk"
            name="milk"
            id="milk"
          />
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
        <button class="btn" @click="addTeaMaker">Add a tea maker</button>
      </div>
      <div v-for="(teaMaker, index) in teaMakers" :key="index">
        {{ teaMaker.name }} -
        {{ teaMaker.milk ? "milk" : "no milk" }}
        {{ teaMaker.sugars < 1 ? "no " : teaMaker.sugars }} sugar
        {{ teaMaker.teaRoundsMade }} rounds made
      </div>
      <div class="flex content-between">
        <button class="btn" @click="pickTeaMaker">Pick a tea maker!</button>
        <button class="btn" @click="clearTeaMakers">Clear</button>
        <button class="btn" @click="resetRounds">Reset rounds</button>
      </div>
      <div>
        <p>{{ selectedTeaMaker }}</p>
      </div>
    </div>
  </div>
</template>

<script setup>
const teaMakers = ref([]);
let selectedTeaMaker = ref("");
import { watch } from "vue";

watch(
  teaMakers,
  (newTeaMakers) => {
    // Update localStorage whenever teaMakers array changes
    localStorage.setItem("teaMakers", JSON.stringify(newTeaMakers));
  },
  { deep: true } // Ensure deep watching for changes inside objects
);

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

    // add teamaker to local Storage
    localStorage.setItem("teaMakers", JSON.stringify(teaMakers.value));
    //reset fields when someone has been added
    teaMaker.name = "";
    teaMaker.sugars = 0;
    teaMaker.milk = false;
  }
};
//clear tea maker array
const clearTeaMakers = () => {
  teaMakers.value.splice(0);
  localStorage.clear();
};

// reset round for each tea maker
const resetRounds = () => {
  teaMakers.value.forEach((teaMaker) => {
    teaMaker.teaRoundsMade = 0;
    localStorage.setItem("teaMakers", JSON.stringify(teaMakers.value));
  });
};
const pickTeaMaker = () => {
  // Ensure there are tea makers available
  if (teaMakers.value.length === 0) {
    selectedTeaMaker.value = "No tea makers available";
    return;
  }
  // Find the minimum number of tea rounds made
  const minRounds = Math.min(
    ...teaMakers.value.map((teaMaker) => teaMaker.teaRoundsMade)
  );

  // Filter tea makers who have made the minimum number of rounds
  const leastPickedTeaMakers = teaMakers.value.filter(
    (teaMaker) => teaMaker.teaRoundsMade === minRounds
  );

  // Randomly select one from the least-picked tea makers
  let chosenTeaMaker =
    leastPickedTeaMakers[
      Math.floor(Math.random() * leastPickedTeaMakers.length)
    ];

  selectedTeaMaker.value = chosenTeaMaker.name;

  // update how many teas have been made
  chosenTeaMaker.teaRoundsMade++;
};

onMounted(() => {
  const storedTeaMakers = localStorage.getItem("teaMakers");
  if (storedTeaMakers) {
    teaMakers.value = JSON.parse(storedTeaMakers);
  }
});
</script>

<style lang="css" scoped>
.btn {
  @apply bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded-full;
}
</style>
