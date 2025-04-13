<template >

<body>
        
    <div class="center">
        <div class="line1">
            <h1>Currency Converter</h1>
        </div>
        <div>
            <div class="orange-label display-inline">
                {{USD}} USD = {{THB}} Bath
            </div>
            <div class="display-inline">
                <button @click="fetch_rates()" > 
                    <!-- <img src="" alt="Refresh" width="20" height="20" /> -->
                    Update Rate</button>
            </div>

        </div>
        <!-- {{rates}} -->
        <div >
            <div class="display-inline">
                
                <label class="topic" for="">Amount</label>
                <div>
                    <input type="number" name="input-one" v-model="input_one" @change="fetch_rates()"/>
                    <select v-model="selectedCurrency">
                        <option 
                            v-for="(rates, code) in rates"
                            :key="code" 
                            :value="code">
                            {{ code }}
                        </option>
                    </select>
                </div>
            
                <!-- <div v-if="selectedCurrency">
                    <p>Rate for {{ selectedCurrency }}: {{ rates[selectedCurrency] }}</p>
                </div> -->
            </div>

            <div class="display-inline">
                <label class="topic" for="">Converted to</label>
                <div>
                    <input type="number" name="input-two" v-model="input_two" placeholder="0"/>
                    <select v-model="selectedCurrency_two">
                        <option 
                            v-for="(rates, code) in rates"
                            :key="code" 
                            :value="code">
                            {{ code }} 
                        </option>
                    </select>
                </div>
            </div>
        </div>
        <button @click="computed" >
            computed
        </button>
        {{result}}
    </div>

</body>

</template>


<script setup>
import {ref, onMounted, computed} from 'vue'
import axios from 'axios'

const rates = ref({})
const USD = ref(null)
const THB = ref(null) 
const sum = ref(null) 

const selectedCurrency = ref('');
const selectedCurrency_two = ref('');

const amountUSD = ref(0);

const input_one = ref(null);
const input_two = ref(null);




const fetch_rates = async () => {
    await axios.get(`${import.meta.env.VITE_API}`)
    .then ((response) => { 
        rates.value = response.data.conversion_rates
        USD.value = response.data.conversion_rates.USD
        THB.value = response.data.conversion_rates.THB

        sum.value = response.data.conversion_rates[input_two.value]

        console.log(rates.value);
        console.log(USD.value);
        console.log(THB.value); 
        console.log(sum.value); 
        console.log(input_two.value); 
    }).catch((err) => {
        console.log(err);
    })
}

// const fetch_data = () => {

// }

const result = computed(() => {
  return (input_one.value * input_two.value).toFixed(2);
  console.log();
});


onMounted(() => fetch_rates())


</script>

<style>


.center {
  margin: auto;
  width: 50%;
  border: 1px solid rgb(208, 198, 198);
  border-radius: 10px;
  padding: 10px;
  margin-top: 5%;
}

.line1 {
    border-bottom: 1px solid rgb(208, 198, 198);
}
button{
    background-color: rgb(232, 102, 22);
    border: 1px solid rgb(232, 102, 22);
    border-radius: 10px;
    margin: 8px;
    padding: 8px;
    color: white;
    padding: 5px;
}

.display-inline{
    display: inline-block;
    width: 48%;
    padding: 10px;
}

input{
    width: 50%;
}
.topic{
 color:  rgb(107, 110, 117);
}

.orange-label{
    background-color: rgb(248, 221, 204);
    color: rgb(232, 102, 22);
    border-radius: 30px;
    width: 30%;
    padding: 5px;
}
</style>