<template>
<div class="random-container">
  Random Activity
	<div v-if="randomActivity">
		<h3>{{randomActivity}}</h3>
	</div>
	<div v-else>
		<h3>{{myError}}</h3>
	</div>
</div>

<div class="filter-container">
		<div class="section">
			<div>Type Filter</div>

			<select name="types" id="types" v-model="selectedType">
				<option v-for="type in types" :key="type" >
					{{type}}
				</option>
			</select>

		</div>

		<div class="section">
			Number of participants:
			<input type="number" v-model="noParticipants" />
		</div>

		<div class="section">
			<b>Price</b>
			<br/>
			Min Price
			<input v-model="minPrice" />

			Max Price
			<input v-model="maxPrice" />
		</div>

		<div class="section">
			<b>Accessibility</b>
			<br/>
			Min Accessibility
			<input v-model="minAccessibility" />

			Max Accessibility
			<input v-model="maxAccessibility">
		</div>

		<button class="submit" @click="fetchFilteredRandomActivity(selectedType, noParticipants, minPrice, maxPrice, minAccessibility, maxAccessibility)">Filter</button>
</div>

</template>

<script>
import axios from "axios"
import { ref } from '@vue/reactivity'
import { onMounted } from '@vue/runtime-core'

export default {
	setup () {
		const randomActivity = ref(null)
		const myError = ref(null)

		const types=ref(["education", "recreational", "social", "diy", "charity", "cooking", "relaxation", "music", "busywork"])

    const minPrice = ref(null)
		const maxPrice = ref(null)

		const minAccessibility = ref(null)
		const maxAccessibility = ref(null)

		const selectedType = ref(null)
    const noParticipants = ref(null)

		const fetchRandomActivity = ((type, participants, minPrice, maxPrice, minAcc, maxAcc) => {
        axios.get('http://www.boredapi.com/api/activity/', {params: {type: type, participants: participants, minPrice: minPrice, maxPrice: maxPrice, minaccessibility: minAcc,
					maxaccessibility: maxAcc}
        })
        .then((data) => {
          randomActivity.value = data.data.activity
					myError.value = data.data.error 
        })
        .catch(function (error) {
          myError.value = "Failed to find an activity. Please refresh the page to find one."
        });
		})

		const fetchFilteredRandomActivity = ((selectedType, noParticipants, minPrice, maxPrice, minAccessibility, maxAccessibility) => {
			fetchRandomActivity(selectedType, noParticipants, minPrice, maxPrice, minAccessibility, maxAccessibility)
		})

		onMounted(() => {
			fetchRandomActivity()
		})

		return {randomActivity, myError, types, noParticipants, minPrice, maxPrice, minAccessibility, maxAccessibility, selectedType, fetchFilteredRandomActivity}

	}
}
</script>

<style>
.section {
	display: flex;
	flex-direction:column;
	align-items: flex-start;
}


.filter-container {
	padding: 50px;
}

.random-container, .filter-container {
    height: 100%;
    display: flex;
    flex-direction: column;
    background: #eee;
    border-radius: 10px;
}

.random-container {
	margin-bottom: 20px;
}

input, select {
  width: 25%;
  padding: 12px 20px;
  margin: 8px 0;
  display: inline-block;
  border: 1px solid #ccc;
  border-radius: 4px;
  box-sizing: border-box;
}

.submit {
  width: 25%;
  background-color: #4CAF50;
  color: white;
  padding: 14px 20px;
  margin: 8px 0;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.submit:hover {
  background-color: #45a049;
}
</style>