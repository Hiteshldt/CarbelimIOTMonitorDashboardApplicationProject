<template>
  <div class="col-md-7 col-12">
    <div :class="['card', 'card-w-background', 'h-100']" :style="{ filter: backgroundFilter }">
      <div class="card-body">
        <h5 class="card-title"><b>India Air Quality Index (AQI)</b></h5>
        <p class="card-text">Real-time PM2.5, PM10 air pollution level in </p>
        <div class="row g-3">
          <div class="col-12 col-lg-6 d-grid justify-content-between">
            <div class="d-lg-flex align-items-center d-none">
              <button id="statusbtn" :class="btnClass">
                {{ simpleAqiStatus }}
              </button>
            </div>
          </div>
          <div class="col-12 col-lg-6 d-flex justify-content-center align-items-end">
            <div>
              <h1 :style="{ color: aqiColor, fontSize: '50px' }">
                {{ aqi || 'N/A' }}
              </h1>
              <p>(AQI-INDIA)</p>
            </div>
            <img id="mascotImg" src="../assets/Mascot.png" :alt="simpleAqiStatus.toLowerCase()" class="img-fluid mb-3" width="63" />
          </div>
          <div class="d-flex align-items-center justify-content-center mt-3 d-lg-none">
            <button :class="btnClass">
              {{ simpleAqiStatus }}
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { computed } from 'vue';
import { useStore } from 'vuex';

// Fetch AQI value from the store
const store = useStore();
const aqi = computed(() => store.getters.toGetAqiIndia?.aqi ?? null);

// Determine AQI status, color, button class, and hue
const getAqiProperties = (value) => {
  if (value === null) return { status: 'N/A', simpleStatus: 'N/A', color: '#b0b0b0', btnClass: 'btn-secondary', filter: 'none' };
  if (value <= 50) return { status: 'Good', simpleStatus: 'Good', color: '#00e400', btnClass: 'btn-success', filter: 'hue-rotate(120deg) saturate(2)' };
  if (value <= 100) return { status: 'Moderate', simpleStatus: 'Moderate', color: '#ffff00', btnClass: 'btn-warning', filter: 'hue-rotate(60deg) saturate(1.5)' };
  if (value <= 150) return { status: 'Unhealthy for Sensitive Groups', simpleStatus: 'Unhealthy', color: '#ff7e00', btnClass: 'btn-orange', filter: 'hue-rotate(30deg) saturate(1.3)' };
  if (value <= 200) return { status: 'Unhealthy', simpleStatus: 'Unhealthy', color: '#ff0000', btnClass: 'btn-danger', filter: 'hue-rotate(0deg) saturate(1.5)' };
  if (value <= 300) return { status: 'Very Unhealthy', simpleStatus: 'Very Unhealthy', color: '#8f3f97', btnClass: 'btn-purple', filter: 'hue-rotate(270deg) saturate(1.5)' };
  return { status: 'Hazardous', simpleStatus: 'Hazardous', color: '#7e0023', btnClass: 'btn-dark-red', filter: 'hue-rotate(300deg) saturate(1.5)' };
};

const aqiProperties = computed(() => getAqiProperties(aqi.value));

const simpleAqiStatus = computed(() => aqiProperties.value.simpleStatus); // Added simple status
const aqiColor = computed(() => aqiProperties.value.color);
const btnClass = computed(() => `btn ${aqiProperties.value.btnClass} text-white rounded-pill px-4 btn-lg`); // Modified for larger size
const backgroundFilter = computed(() => aqiProperties.value.filter);
</script>

<style scoped>
#statusbtn{
  margin-left: 40px;
}

#mascotImg {
  margin-left: 20px;
  width: 100px;
}

.card-w-background {
  background-image: url('../assets/india_map_shape_new_3.png');
  background-position: bottom;
  background-size: 100% 95px;
  background-repeat: no-repeat;
  border: none;
}

.air-quality-card {
  background-image: url('../assets/air-quality-map.png');
  background-size: cover;
  border: none;
}

.air-quality-card h2 {
  font-size: 25px;
  font-weight: 600;
  padding-top: 25px;
  color: #fafbfc;
}

.air-quality-card h5 {
  font-size: 20px;
  font-weight: 500;
  color: #fafbfc;
}

.air-quality-card button {
  background-color: #31b66f;
}

.btn-success {
  background-color: #00e400 !important;
}

.btn-warning {
  background-color: #ffff00 !important;
}

.btn-orange {
  background-color: #ff7e00 !important;
}

.btn-danger {
  background-color: #ff0000 !important;
}

.btn-purple {
  background-color: #8f3f97 !important;
}

.btn-dark-red {
  background-color: #7e0023 !important;
}

/* Added styles for larger button */
.btn-lg {
  font-size: 25px;
  font-weight: bold;
  padding: 10px 20px;
}
</style>
