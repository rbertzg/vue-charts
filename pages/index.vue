<template>
  <div>
    <Bar :data="chartData" :options="{ indexAxis: 'y' }" />
  </div>
</template>

<script setup lang="ts">
import { type ChartData } from "chart.js";
import { Bar } from "vue-chartjs";

type Movie = {
  title: string;
  rating: number;
};

const movies = ref<Movie[]>([]);

const chartData = computed((): ChartData<"bar"> => {
  return {
    labels: movies.value.map((movie) => movie.title),
    datasets: [
      {
        label: "IMDb rating",
        data: movies.value.map((movie) => movie.rating),
        backgroundColor: "#c82834",
        hoverBackgroundColor: "#d93945",
        stack: "rating",
      },
      {
        label: "My rating",
        data: movies.value.map((movie) => movie.rating + 1),
        backgroundColor: "#244771",
        hoverBackgroundColor: "#355882",
        stack: "rating",
      },
    ],
  };
});

const fetchData = async () => {
  const res = await fetch("/api.json");
  const data = await res.json();
  return data;
};

movies.value = await fetchData();
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
