<template>
    <div class="p-6">
        <h1 class="text-6xl font-semibold mb-4">Pizza Menu</h1>
        <hr class="mx-4" />

        <div class="grid grid-cols-2 gap-4 mt-4">
            <div>
                <div>
                    <h3 class="text-2xl font-semibold">Pizza Size</h3>
                    <button
                        type="button"
                        @click="selectSize('small')"
                        class="text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-6 me-2 mb-2 dark:bg-blue-600 dark:hover:bg-blue-700 focus:outline-none dark:focus:ring-blue-800"
                    >
                        <span class="text-xl">Small Pizza</span>
                    </button>
                    <button
                        type="button"
                        @click="selectSize('medium')"
                        class="text-white bg-yellow-700 hover:bg-blue-800 focus:ring-4 focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-6 me-2 mb-2 dark:bg-blue-600 dark:hover:bg-blue-700 focus:outline-none dark:focus:ring-blue-800"
                    >
                        <span class="text-xl">Medium Pizza</span>
                    </button>
                    <button
                        type="button"
                        @click="selectSize('large')"
                        class="text-white bg-green-700 hover:bg-blue-800 focus:ring-4 focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-6 me-2 mb-2 dark:bg-blue-600 dark:hover:bg-blue-700 focus:outline-none dark:focus:ring-blue-800"
                    >
                        <span class="text-xl">Large Pizza</span>
                    </button>
                </div>
                <div>
                    <h3 class="text-2xl font-semibold my-4">Options</h3>
                    <p class="text-lg font-semibold" v-if="selectedPizza.size">
                        You have selected size {{ selectedPizza.size }} pizza
                    </p>
                    <div class="mt-2">
                        <div v-if="selectedPizza.size != 'large'">
                            <p class="text-lg mb-2">
                                Would you like pepperoni on your pizza?
                            </p>
                            <button
                                type="button"
                                @click="addPepperoni('yes')"
                                class="text-white bg-green-700 hover:bg-blue-800 focus:ring-4 focus:ring-blue-300 font-medium rounded-lg text-sm p-5 me-2 mb-2 dark:bg-blue-600 dark:hover:bg-blue-700 focus:outline-none dark:focus:ring-blue-800"
                            >
                                YES
                            </button>
                            <button
                                type="button"
                                @click="addPepperoni('no')"
                                class="text-white bg-red-700 hover:bg-blue-800 focus:ring-4 focus:ring-blue-300 font-medium rounded-lg text-sm p-5 me-2 mb-2 dark:bg-blue-600 dark:hover:bg-blue-700 focus:outline-none dark:focus:ring-blue-800"
                            >
                                NO
                            </button>
                        </div>
                        <div v-else class="text-lg mb-2">
                            <p>Large Pizza can't have pepperoni topping.</p>
                        </div>
                    </div>

                    <div class="mt-2">
                        <p class="text-lg mb-2">Would you like extra cheese?</p>
                        <button
                            type="button"
                            @click="addCheese('yes')"
                            class="text-white bg-green-700 hover:bg-blue-800 focus:ring-4 focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-5 me-2 mb-2 dark:bg-blue-600 dark:hover:bg-blue-700 focus:outline-none dark:focus:ring-blue-800"
                        >
                            YES</button
                        ><button
                            @click="addCheese('no')"
                            type="button"
                            class="text-white bg-red-700 hover:bg-blue-800 focus:ring-4 focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-5 me-2 mb-2 dark:bg-blue-600 dark:hover:bg-blue-700 focus:outline-none dark:focus:ring-blue-800"
                        >
                            NO
                        </button>
                    </div>
                </div>
                <button
                    type="button"
                    @click="confirmPizza"
                    class="text-white mt-4 bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-5 me-2 mb-2 dark:bg-blue-600 dark:hover:bg-blue-700 focus:outline-none dark:focus:ring-blue-800"
                >
                    ADD
                </button>
            </div>
            <div>
                <h3 class="text-2xl font-semibold">Bill</h3>
                <ul>
                    <li v-for="(pizza, index) in pizzas" :key="index">
                        <div class="flex justify-between">
                            <p>
                                {{ pizza.size }} Pizza
                                <span
                                    v-if="pizza.pepperoni"
                                    class="font-semibold"
                                    >+ pepperoni </span
                                ><span
                                    class="font-semibold"
                                    v-if="pizza.extraCheese"
                                    >+ extra cheese</span
                                >
                            </p>
                            <p>
                                {{
                                    new Intl.NumberFormat("ms-MY", {
                                        style: "currency",
                                        currency: "MYR",
                                    }).format(pizza.pizzaPrice)
                                }}
                            </p>
                        </div>
                    </li>
                    <li>
                        <div class="flex justify-between">
                            <p class="font-bold">Total</p>
                            <p class="font-bold">
                                {{
                                    new Intl.NumberFormat("ms-MY", {
                                        style: "currency",
                                        currency: "MYR",
                                    }).format(totalPrice)
                                }}
                            </p>
                        </div>
                    </li>
                </ul>
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
const totalPrice = ref(0.0);

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
