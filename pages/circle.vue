<template>
  <div style="display: flex; justify-content: center">
    <Pie
      :data="chartData"
      :options="{
        cutout: '40%',
      }"
      style="height: 600px"
    />
  </div>
</template>

<script setup lang="ts">
import type { ChartData } from "chart.js";
import { Pie } from "vue-chartjs";

const repos = [
  "vuejs/vue",
  "facebook/react",
  "angular/angular",
  "sveltejs/svelte",
  "solidjs/solid",
];

const repoData = ref<any[]>([]);

const chartData = computed((): ChartData<"pie"> => {
  return {
    labels: repos,
    datasets: [
      {
        label: "Stars",
        data: repoData.value.map((repo) => repo.stargazers_count),
        backgroundColor: [
          "#41B883",
          "#149eca",
          "#c30e2e",
          "#f96743",
          "#043271",
        ],
      },
    ],
  };
});

const fetchData = async () => {
  for await (const repo of repos) {
    const res = await fetch(`https://api.github.com/repos/${repo}`);
    const data = await res.json();
    repoData.value.push(data);
  }
};
fetchData();
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
