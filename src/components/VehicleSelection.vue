<script>
import axios from 'axios'

export default {
  data() {
    return {
      years: [],
      selectedYear: '',
      loadingYears: false,

      makes: [],
      selectedMake: '',
      loadingMakes: false,

      models: [],
      selectedModel: '',
      loadingModels: false,
    }
  },
  mounted() {
    console.log('MOUNTED')
    this.fetchYears()
  },
  methods: {
    async fetchYears() {
      try {
        const response = await axios.get(
          `/api/v2/vehicles/years/?token=5cbe12fb62f4941267d623499a2a4fd5948fd3ef`,
          {
            headers: {
              Accept: 'application/json',
              'Content-Type': 'application/json',
            },
          },
        )
        this.years = response.data
      } catch (error) {
        console.error('Error fetching years:', error)
      }
    },

    async fetchMakes() {
      if (!this.selectedYear) return
      this.loadingMakes = true
      try {
        const response = await axios.get(
          `/api/v2/vehicles/makes/?year=${this.selectedYear}&token=5cbe12fb62f4941267d623499a2a4fd5948fd3ef`,
          {
            headers: {
              Accept: 'application/json',
              'Content-Type': 'application/json',
            },
          },
        )

        this.makes = response.data.map((item) => item.make)
        this.selectedMake = ''
        this.models = []
        this.selectedModel = ''
      } catch (error) {
        console.error('Error fetching makes:', error)
      } finally {
        this.loadingMakes = false
      }
    },

    async fetchModels() {
      if (!this.selectedYear || !this.selectedMake) return
      this.loadingModels = true
      try {
        const response = await axios.get(
          `/api/v2/vehicles/models/?year=${this.selectedYear}&make=${this.selectedMake}&token=5cbe12fb62f4941267d623499a2a4fd5948fd3ef`,
          {
            headers: {
              Accept: 'application/json',
              'Content-Type': 'application/json',
            },
          },
        )

        this.models = response.data.map((item) => item.model)
        this.selectedModel = ''
      } catch (error) {
        console.error('Error fetching models:', error)
      } finally {
        this.loadingModels = false
      }
    },
  },
}
</script>

<template>
  <div class="page-container">
    <div class="container">
      <h1>Vehicle Selection</h1>

      <div class="dropdowns">
        <div class="dropdown">
          <label for="year">Year:</label>
          <select
            v-model="selectedYear"
            :disabled="loadingYears || years.length === 0"
            @change="fetchMakes"
          >
            <option value="" disabled selected>Select Year</option>
            <option v-for="year in years" :key="year.year" :value="year.year">
              {{ year.year }}
            </option>
          </select>
        </div>

        <div class="dropdown">
          <label for="make">Make:</label>
          <select
            v-model="selectedMake"
            :disabled="loadingMakes || makes.length === 0"
            @change="fetchModels"
          >
            <option value="" disabled selected>Select Make</option>
            <option v-for="make in makes" :key="make" :value="make">
              {{ make }}
            </option>
          </select>
        </div>

        <div class="dropdown">
          <label for="model">Model:</label>
          <select v-model="selectedModel" :disabled="loadingModels || models.length === 0">
            <option value="" disabled selected>Select Model</option>
            <option v-for="model in models" :key="model" :value="model">
              {{ model }}
            </option>
          </select>
        </div>
      </div>
    </div>
  </div>
</template>

<style>
.page-container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  margin: 0;
  width: 200%;
}

.container {
  background-color: #ffffff8a;
  padding: 30px 40px;
  border-radius: 10px;
  box-shadow: 0 4px 12px rgba(160, 158, 158, 0.113);
  width: 150%;
  max-width: 800px;
  text-align: center;
}

h1 {
  font-size: 28px;
  margin-bottom: 20px;
  color: #333;
}

.dropdowns {
  display: flex;
  justify-content: space-between;
  gap: 20px;
  flex-wrap: wrap;
  margin-top: 20px;
}

.dropdown {
  display: flex;
  flex-direction: column;
  flex-grow: 1;
  min-width: 180px;
}

label {
  margin-bottom: 5px;
  font-weight: bold;
  color: #333;
}

select {
  padding: 10px;
  font-size: 16px;
  border: 1px solid #ddd;
  border-radius: 5px;
  background-color: #fff;
  cursor: pointer;
  width: 100%;
}

select:disabled {
  background-color: #e0e0e0;
}

select:focus {
  outline: none;
  border-color: #3103bb;
}
</style>
