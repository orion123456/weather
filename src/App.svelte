<script>
	import moment from "moment";
	import 'moment/locale/ru'

	//Изменить код так чтобы при загрузке страницы данные сразу же отображались
	//После отправки формы тоже
	//Вывести день недели и время
	//Почитать про промисы и про async await https://www.youtube.com/watch?v=5kAPExqSZ1I
	//Вывести время в зависимости от таймзоны timezone

	const appid = '77d3010e8f2da851498d867aa5c6e12f'
	const units = 'metric'
	const lang = 'ru'

	let lat = '56.857166'
	let lon = '35.912293'

	const endpoint = "https://api.openweathermap.org/data/2.5/weather";

	let temp
	let weather = []
	let name

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
					console.log(data)
				});
	}

	function handleSubmit(event) {
		const value = event.currentTarget.coords.value
		let value1 = value.split(', ')
		lat = value1[0]
		lon = value1[1]

		getWeather()
	}

	getWeather()

	let weekTime = moment().locale('ru').format('dddd, HH:mm')

	let timeZone = moment().utcOffset(180);
	console.log(timeZone)

</script>

<h1>Виджет погоды</h1>
<form on:submit|preventDefault={handleSubmit}>
	<input type="text" name="coords">
	<button>Получить погоду</button>
</form>


<div>{name}</div>
<div>{weekTime}</div>
<div>{temp}°C</div>
{#each weather as item}
	<div>{item.description}</div>
{/each}



<style>

</style>