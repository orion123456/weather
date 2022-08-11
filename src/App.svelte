<script>
	import moment from "moment";
	import 'moment/locale/ru'

	//Почитать про промисы и про async await https://www.youtube.com/watch?v=5kAPExqSZ1I


	const appid = '77d3010e8f2da851498d867aa5c6e12f'
	const units = 'metric'
	const lang = 'ru'

	let lat = '56.857166'
	let lon = '35.912293'

	const endpoint = "https://api.openweathermap.org/data/2.5/weather";

	let temp
	let weather = []
	let name

	let value = `${lat}, ${lon}`

	let timeZone

	function getWeather() {
		const url = `${endpoint}?appid=${appid}&units=${units}&lang=${lang}&lat=${lat}&lon=${lon}`

		fetch(url)
				.then((response) => {
					return response.json();
				})
				.then((data) => {
					temp = data.main.temp.toFixed(0)
					weather = data.weather
					name = data.name
					timeZone = moment().utcOffset(data.timezone/60).format('dddd, HH:mm')
				});
	}

	function handleSubmit() {
		let value1 = value.split(', ')
		lat = value1[0]
		lon = value1[1]

		getWeather()

	}

	getWeather()

</script>

<h1>Виджет погоды</h1>
<form on:submit|preventDefault={handleSubmit}>
	<input type="text" name="coords" bind:value>
	<button>Получить погоду</button>
</form>

<div>{name}</div>
<div>{timeZone}</div>
<div>{temp}°C</div>
{#each weather as item}
	<div>{item.description}</div>
{/each}

<style>

</style>