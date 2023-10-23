<script>
import axios from 'axios';
import { ref, watch } from 'vue';

export default{
    setup() {
        const warehouses = ref([
            {
                SiteKey: "",
                Description: "",
                CityDescription: ""
            }
        ]);
        const city = ref("");

        watch(city, (city) => {
            axios.post("https://api.novaposhta.ua/v2.0/json/", {
                "apiKey": "059bb4c038ff82d708e48a488c043cf9",
                "modelName": "Address",
                "calledMethod": "getWarehouses",
                "methodProperties": {
                    "CityName": city
                }
            })
            .then(response => {
                console.log(response);
                warehouses.value = response.data.data;
            })
        });

        return { warehouses, city };
    }
}
</script>

<template>

<div>
    <label class="label-city">Населений пункт</label><br>
    <input type="text" class="input input-city" v-model.lazy="city"><br>
    <label class="label-warehouse">Поштове відділення</label><br>
    <select class="select select-warehouse">
        <option v-for="wh in warehouses" v-bind:key="wh.SiteKey">{{wh.Description}}</option>
    </select>
</div>

</template>

<style scoped>
.input, .select{
    margin-top: 15px;
    margin-bottom: 25px;
    padding: 20px 10px;
    width: 600px;
}
</style>
