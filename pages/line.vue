<template>
  <div>
    <Line :data="chartData" />
  </div>
</template>

<script setup lang="ts">
import { type ChartData } from "chart.js";
import { Line } from "vue-chartjs";

type Movie = {
  title: string;
  rating: number;
};

const weatherData = ref();

const chartData = computed((): ChartData<"line"> => {
  return {
    labels: weatherData.value.hourly.time.map(
      (timestamp: string) => useDateFormat(timestamp, "dddd h aa").value
    ),
    datasets: [
      {
        label: "Temperature",
        data: weatherData.value.hourly.temperature_2m,
        backgroundColor: "#c82834",
        hoverBackgroundColor: "#d93945",
      },
    ],
  };
});

const fetchData = async () => {
  const res = await fetch(
    "https://api.open-meteo.com/v1/forecast?latitude=52.52&longitude=13.41&current=temperature_2m,wind_speed_10m&hourly=temperature_2m,relative_humidity_2m,wind_speed_10m"
  );
  const data = await res.json();
  return data;
};

weatherData.value = await fetchData();
</script>

<style>
html {
  font-size: 62.5%;
}

body {
  font-size: 1.6rem;
  padding: 5rem;
}
</style>
