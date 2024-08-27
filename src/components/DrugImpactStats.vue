<template>
  <div class="drug-impact-stats">
    <h1>Counterfeit Drugs Impact in Iran</h1>
    <div class="stats">
      <div class="time-stats-group">
        <!-- First pair of elapsed time stats -->
        <div class="stat-item">
          <div class="stat-label">Elapsed Time Since Page Load:</div>
          <div class="stat-value">{{ elapsedTimeSincePageLoadFormatted }}</div>
          <!-- PNG icon next to the elapsed time -->
          <!-- <img src="../assets/alarm-clock.png" alt="Time Icon" class="time-icon" /> -->
        </div>
        <div>
          <img src="../assets/alarm-clock.png" alt="Time Icon" class="time-icon" style="margin-top: 25px;" />
        </div>
        <div class="stat-item">
          <div class="stat-label">Elapsed Time Since Start:</div>
          <div class="stat-value">{{ elapsedTimeSinceStartFormatted }}</div>
          <!-- <img src="../assets/alarm-clock.png" alt="Time Icon" class="time-icon" /> -->
        </div>
      </div>

      <!-- Category stats grouped two by two in each iteration -->
      <template v-for="(category, index) in categories" :key="category.name">
        <div v-if="index % 2 === 0" class="category-pair">
          <div class="category-section">
            <img src="../assets/earth.png" alt="Time Icon" class="time-icon" />
            <h2>{{ category.label }} </h2>
            <div class="stat-item">
              <img src="../assets/patient.png" alt="Time Icon" class="time-icon" />
              <div class="stat-label">Patients:</div>
              <div class="stat-value">{{ formatNumber(category.patients) }}</div>
            </div>
            <div class="stat-item">
              <img src="../assets/cash.png" alt="Time Icon" class="time-icon" />
              <div class="stat-label">Financial Impact:</div>
              <div class="stat-value">{{ formatCurrency(category.financialImpact) }}</div>
            </div>
            <div class="stat-item">
              <img src="../assets/cash.png" alt="Time Icon" class="time-icon" />
              <div class="stat-label">Financial Impact Since Start:</div>
              <div class="stat-value">{{ formatCurrency(category.financialImpactSinceStart) }}</div>
            </div>
            <div class="stat-item">
              <img src="../assets/cash.png" alt="Time Icon" class="time-icon" />
              <div class="stat-label">Financial Impact Since Start:</div>
              <div class="stat-value">{{ formatCurrency(category.additionalNumber) }}</div>
            </div>
          </div>
          <!-- Pair with next category if exists -->
          <div v-if="categories[index + 1]" class="category-section">
            <img src="../assets/earth.png" alt="Time Icon" class="time-icon" />
            <h2>{{ categories[index + 1].label }}</h2>
            <div class="stat-item">
              <img src="../assets/patient.png" alt="Time Icon" class="time-icon" />
              <div class="stat-label">Patients:</div>
              <div class="stat-value">{{ formatNumber(categories[index + 1].patients) }}</div>
            </div>
            <div class="stat-item">
              <img src="../assets/cash.png" alt="Time Icon" class="time-icon" />
              <div class="stat-label">Financial Impact:</div>
              <div class="stat-value">{{ formatCurrency(categories[index + 1].financialImpact) }}</div>
            </div>
            <div class="stat-item">
              <img src="../assets/cash.png" alt="Time Icon" class="time-icon" />
              <div class="stat-label">Financial Impact Since Start:</div>
              <div class="stat-value">{{ formatCurrency(categories[index + 1].financialImpactSinceStart) }}</div>
            </div>
            <div class="stat-item">
              <img src="../assets/cash.png" alt="Time Icon" class="time-icon" />
              <div class="stat-label">Financial Impact Since Start:</div>
              <div class="stat-value">{{ formatCurrency(categories[index + 1].additionalNumber) }}</div>
            </div>
          </div>
        </div>
      </template>
    </div>
  </div>
</template>






<script setup>
import { ref, reactive, computed, onMounted } from 'vue';

const pageLoadTime = ref(new Date());
const startTime = new Date('2023-01-01T00:00:00');
const now = ref(new Date());

const categories = reactive([
  {
    name: 'lessDeveloped',
    label: 'Less Developed Countries',
    patients: 0,
    financialImpact: 0,
    financialImpactSinceStart: 0,
    additionalNumber: 0,
    multiplier: 14,
  },
  {
    name: 'developing',
    label: 'Developing Countries',
    patients: 0,
    financialImpact: 0,
    financialImpactSinceStart: 0,
    additionalNumber: 0,
    multiplier: 2.79,
  },
  {
    name: 'developed',
    label: 'Developed Countries',
    patients: 0,
    financialImpact: 0,
    financialImpactSinceStart: 0,
    additionalNumber: 0,
    multiplier: 0.83,
  },
  {
    name: 'usa',
    label: 'USA',
    patients: 0,
    financialImpact: 0,
    financialImpactSinceStart: 0,
    additionalNumber: 0,
    multiplier: 0.28,
  },
]);

const updateStatistics = () => {
  const elapsedSecondsSinceStart = (now.value.getTime() - startTime.getTime()) / 1000;
  const elapsedSecondsSincePageLoad = (now.value.getTime() - pageLoadTime.value.getTime()) / 1000;

  categories.forEach(category => {
    category.patients = Math.floor(elapsedSecondsSincePageLoad * category.multiplier * 5); // Adjusted for page load time
    category.financialImpact = Math.floor(elapsedSecondsSincePageLoad * category.multiplier * 100000); // Adjusted for page load time
    category.financialImpactSinceStart = Math.floor(elapsedSecondsSinceStart * category.multiplier * 100); // Keeps original start time calculation
    category.additionalNumber = Math.floor(elapsedSecondsSinceStart * category.multiplier * 1000); // Keeps original start time calculation
  });

  now.value = new Date(); // Update the current time
};

onMounted(() => {
  setInterval(updateStatistics, 1000); // Update statistics every second
});

const elapsedTimeSincePageLoadFormatted = computed(() => {
  const elapsed = new Date(now.value - pageLoadTime.value);
  return `${elapsed.getUTCHours()}h ${elapsed.getUTCMinutes()}m ${elapsed.getUTCSeconds()}s`;
});

const elapsedTimeSinceStartFormatted = computed(() => {
  const elapsed = new Date(now.value - startTime);
  return `${elapsed.getUTCHours()}h ${elapsed.getUTCMinutes()}m ${elapsed.getUTCSeconds()}s`;
});

const formatNumber = number => new Intl.NumberFormat('en-US').format(number);
const formatCurrency = number => number.toLocaleString('en-US', { style: 'currency', currency: 'USD' });

const navigateToDetail = categoryName => {
  console.log(`Navigate to details about: ${categoryName}`);
};
</script>
<style>
.drug-impact-stats {
  width: 180vh;
  margin: 20px auto;
  padding: 20px;
  /* From https://css.glass */
  background: rgba(255, 255, 255, 0.17);
  border-radius: 16px;
  box-shadow: 0 4px 30px rgba(0, 0, 0, 0.1);
  backdrop-filter: blur(3.5px);
  -webkit-backdrop-filter: blur(3.5px);

}

h1,
h2 {
  color: #333;
  text-align: center;
  margin-bottom: 20px;
}

.stats {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.stat-item {
  /* From https://css.glass */
  background: rgba(255, 255, 255, 0.36);
  border-radius: 16px;
  box-shadow: 0 4px 30px rgba(0, 0, 0, 0.1);
  backdrop-filter: blur(5.4px);
  -webkit-backdrop-filter: blur(5.4px);
  border-radius: 8px;
  padding: 20px;
  margin: 10px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.08);
  cursor: pointer;
  transition: transform 0.3s ease-in-out, box-shadow 0.3s ease-in-out;
}

.stat-item:hover {
  transform: translateY(-5px);
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.16);
}

.stat-label {
  font-size: 16px;
  color: #666;
  margin-bottom: 5px;
}

.stat-value {
  font-size: 20px;
  font-weight: 600;
  color: #333;
}

/* .category-section {
  border: 1px solid #ccc;
  padding: 10px;
  border-radius: 8px;
  margin-bottom: 20px;
} */





.time-stats-group,
.category-pair {
  display: flex;
  justify-content: space-between;
  margin-bottom: 20px;
  /* Space below each group */
}

.stat-item {
  /* margin-right: 0px; */
  flex-basis: 50%;
  /* Gives each stat item an equal width within its container */
}

/* .stat-item:last-child {
  margin-right: 0;
} */

.category-section {
  flex: 1;
  padding-right: 20px;
  /* Padding to the right of each category section */
}

.category-section:last-child {
  padding-right: 0;
  /* Remove padding from the last category section in each pair */
}

.category-pair {
  padding-bottom: 20px;
  /* Additional padding below each pair */
  border-bottom: 1px solid #eee;
  /* Optional line to visually separate pairs */
}

.category-pair:last-of-type {
  border-bottom: none;
  /* Remove bottom border from the last pair */
}

/* Additional specific padding between Developing Countries and Less Developed Countries */
.category-section:nth-child(2) {
  padding-left: 10px;
  /* Adds space to the left of Developing Countries if it follows Less Developed Countries */
  padding-right: 10px;
}

.stats {
  display: flex;
  flex-direction: column;
}

.stat-item {
  display: flex;
  justify-content: space-between;
  margin-bottom: 10px;
}

.stat-label,
.stat-value {
  flex: 1;
  /* Ensures that both label and value take up equal space */
}

.category-pair {
  display: flex;
  justify-content: space-between;
  margin-bottom: 20px;
}

.category-section {
  flex: 1;
  margin-right: 20px;
  /* Space between paired categories */
}

.category-section:last-child {
  margin-right: 0;
  /* Remove right margin on the last category in each pair */
}
</style>
