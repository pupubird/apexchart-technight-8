<script>
	import ApexCharts from "apexcharts";
	import { onMount } from "svelte";

	let data;
	onMount(async () => {
		let response = await fetch("/student_perfomance.json");
		data = await response.json();
		gender();
		math_scores();
	});

	function gender() {
		let gender_series = data.map((d) => d.gender);

		let male_count = gender_series.reduce((arr, current) => {
			if (current == "male") {
				return ++arr;
			}
			return arr;
		}, 0);

		let female_count = gender_series.reduce((arr, current) => {
			if (current == "female") {
				return ++arr;
			}
			return arr;
		}, 0);

		let options = {
			chart: {
				type: "donut",
			},
			series: [female_count, male_count],
			labels: ["female", "male"],
			title: {
				text: "Gender",
			},
		};
		let chart = new ApexCharts(document.querySelector("#gender"), options);
		chart.render();
	}

	function math_scores() {
		let scores = data.map((d) => d["math score"]);

		let _0_20 = scores.filter((d) => d < 20 && d >= 0).length;
		let _20_40 = scores.filter((d) => d < 40 && d >= 20).length;
		let _40_60 = scores.filter((d) => d < 60 && d >= 40).length;
		let _60_80 = scores.filter((d) => d < 80 && d >= 60).length;
		let _80_100 = scores.filter((d) => d < 100 && d >= 80).length;

		let options = {
			chart: {
				type: "bar",
			},
			series: [
				{
					data: [_0_20, _20_40, _40_60, _60_80, _80_100],
				},
			],
			xaxis: {
				categories: ["0-20", "20-40", "40-60", "60-80", "80-100"],
			},
			title: {
				text: "Math scores",
			},
		};
		let chart = new ApexCharts(document.querySelector("#math_scores"), options);
		chart.render();
	}
</script>

<style>
	div {
		width: 500px;
	}
</style>

<div id="gender" />
<div id="math_scores" />
