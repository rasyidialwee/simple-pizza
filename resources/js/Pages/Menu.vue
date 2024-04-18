<template>
    <div>
        <h1>Pizza Menu</h1>

        <div class="grid grid-cols-2 gap-4">
            <div>
                <div>
                    <button
                        type="button"
                        @click="selectSize('small')"
                        class="text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-2.5 me-2 mb-2 dark:bg-blue-600 dark:hover:bg-blue-700 focus:outline-none dark:focus:ring-blue-800"
                    >
                        Small Pizza
                    </button>
                    <button
                        type="button"
                        @click="selectSize('medium')"
                        class="text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-2.5 me-2 mb-2 dark:bg-blue-600 dark:hover:bg-blue-700 focus:outline-none dark:focus:ring-blue-800"
                    >
                        Medium Pizza
                    </button>
                    <button
                        type="button"
                        @click="selectSize('large')"
                        class="text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-2.5 me-2 mb-2 dark:bg-blue-600 dark:hover:bg-blue-700 focus:outline-none dark:focus:ring-blue-800"
                    >
                        Large Pizza
                    </button>
                </div>
                <div>
                    <h4>Options</h4>
                    <p>You have selected size {{ selectedPizza.size }} pizza</p>
                    <div>
                        <div v-if="selectedPizza.size != 'large'">
                            <p>Would you like pepperoni on your pizza?</p>
                            <button
                                type="button"
                                @click="addPepperoni('yes')"
                                class="text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-2.5 me-2 mb-2 dark:bg-blue-600 dark:hover:bg-blue-700 focus:outline-none dark:focus:ring-blue-800"
                            >
                                Yes</button
                            ><button
                                type="button"
                                @click="addPepperoni('no')"
                                class="text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-2.5 me-2 mb-2 dark:bg-blue-600 dark:hover:bg-blue-700 focus:outline-none dark:focus:ring-blue-800"
                            >
                                No
                            </button>
                        </div>
                        <div v-else>
                            <p>Large Pizza can't have pepperoni topping.</p>
                        </div>
                    </div>

                    <div>
                        <p>Would you like extra cheese?</p>
                        <button
                            type="button"
                            @click="addCheese('yes')"
                            class="text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-2.5 me-2 mb-2 dark:bg-blue-600 dark:hover:bg-blue-700 focus:outline-none dark:focus:ring-blue-800"
                        >
                            Yes</button
                        ><button
                            @click="addCheese('no')"
                            type="button"
                            class="text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-2.5 me-2 mb-2 dark:bg-blue-600 dark:hover:bg-blue-700 focus:outline-none dark:focus:ring-blue-800"
                        >
                            No
                        </button>
                    </div>
                </div>
                <button
                    type="button"
                    @click="confirmPizza"
                    class="text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-2.5 me-2 mb-2 dark:bg-blue-600 dark:hover:bg-blue-700 focus:outline-none dark:focus:ring-blue-800"
                >
                    Confirm
                </button>
            </div>
            <div>
                <h3>Bill</h3>
                {{ selectedPizza }}
                <br />
                {{ pizzas }}
            </div>
        </div>
    </div>
</template>

<script setup>
import { ref } from "vue";

const selectedPizza = ref({
    size: "",
    pepperoni: false,
    extraCheese: false,
    pizzaPrice: 0,
});

const pizzas = ref([]);
const totalPrice = ref(0);

const selectSize = (size) => {
    selectedPizza.value.size = size;
    selectedPizza.value.pizzaPrice =
        size === "small" ? 15 : size === "medium" ? 22 : 30;
};

const addPepperoni = (value) => {
    if (value === "yes" && !selectedPizza.value.pepperoni) {
        selectedPizza.value.pepperoni = true;
        if (selectedPizza.value.size === "small") {
            selectedPizza.value.pizzaPrice += 3;
        } else if (selectedPizza.value.size === "medium") {
            selectedPizza.value.pizzaPrice += 5;
        }
    } else if (value === "no" && selectedPizza.value.pepperoni) {
        selectedPizza.value.pepperoni = false;
        if (selectedPizza.value.size === "small") {
            selectedPizza.value.pizzaPrice -= 3;
        } else if (selectedPizza.value.size === "medium") {
            selectedPizza.value.pizzaPrice -= 5;
        }
    }
};

const addCheese = (value) => {
    if (value === "yes" && !selectedPizza.value.extraCheese) {
        selectedPizza.value.extraCheese = true;
        selectedPizza.value.pizzaPrice += 6;
    } else if (value === "no" && selectedPizza.value.extraCheese) {
        selectedPizza.value.extraCheese = false;
        selectedPizza.value.pizzaPrice -= 6;
    }
};

const confirmPizza = () => {
    pizzas.value.push(selectedPizza.value);
    totalPrice.value += selectedPizza.value.pizzaPrice;
    selectedPizza.value = {
        size: "",
        pepperoni: false,
        extraCheese: false,
        pizzaPrice: 0,
    };
};
</script>
