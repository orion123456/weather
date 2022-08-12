<script>
	import moment from "moment";
	import 'moment/locale/ru'

	const appid = '77d3010e8f2da851498d867aa5c6e12f'
	const units = 'metric'
	const lang = 'ru'
	const endpoint = "https://api.openweathermap.org/data/2.5/weather";
	let temp
	let weather = []
	let name
	let feelsLike
	let timeZone
	let coords = '55.755819, 37.617644'
	let skeleton = true

	navigator.geolocation.getCurrentPosition((position) => {
		const lat  = position.coords.latitude;
		const lon = position.coords.longitude;
		coords = `${lat}, ${lon}`
		getWeather()
	})

	function getWeather() {
		const value = coords.split(', ')
		const lat = value[0]
		const lon = value[1]
		const url = `${endpoint}?appid=${appid}&units=${units}&lang=${lang}&lat=${lat}&lon=${lon}`

		fetch(url)
				.then((response) => {
					return response.json();
				})
				.then((data) => {
					skeleton = false
					temp = data.main.temp.toFixed(0)
					weather = data.weather
					name = data.name
					feelsLike = data.main.feels_like.toFixed(0)
					timeZone = moment().utcOffset(data.timezone/60).format('dddd, HH:mm')
					console.log(data)
				});
	}

	getWeather()


</script>

<div class="wrapper">
	<div class="main-block">
		<div class="left-block">
			<div class="city" class:skeleton={skeleton}>{name}</div>
			{#each weather as item}
				<div class="weather-info" class:skeleton={skeleton}>{item.description}</div>
			{/each}
			<div class="weather-img" class:skeleton={skeleton}>
				{#each weather as item}
					<img src="icons/{item.icon}.svg" alt="weather" class="img">
				{/each}
			</div>
			<div class="temp" class:skeleton={skeleton}>{temp}°C</div>
			<div class="feels-like" class:skeleton={skeleton}>Ощущается как {feelsLike}</div>
		</div>
		<div class="right-block">
			<div class="top-block">
				<div class="time-zone" class:skeleton={skeleton}>{timeZone}</div>
				<form on:submit|preventDefault={getWeather}>
					<input type="text" name="coords" bind:value={coords}>
					<button>Получить погоду</button>
				</form>
			</div>



		</div>
	</div>
</div>




<style>
	.wrapper {
		min-height: 100vh;
		background-image: radial-gradient(circle 993px at .5% 50.5%,rgba(137,171,245,.37) 0,#f5f7fc 100.2%);
		display: flex;
		align-items: center;
		justify-content: center;
	}
	.main-block {
		width: 100%;
		max-width: 1200px;
		background: hsla(0,0%,100%,.95);
		box-shadow: 0 8px 32px 0 rgb(83 89 179 / 37%);
		backdrop-filter: blur(3px);
		border-radius: 30px;
		overflow: hidden;
		display: flex;
	}
	.left-block {
		text-align: center;
		padding: 30px;
		width: 30%;
	}
	.right-block {
		width: 70%;
		background-color: #f7f7f7;
		padding: 30px;
	}
	.city {
		font-size: 38px;
		line-height: 43px;
		font-weight: bold;
	}
	.weather-info {
		font-size: 24px;
		line-height: 30px;
		margin-top: 10px;
	}
	.weather-img {
		height: 250px;
		line-height: 0;
		margin-top: 20px;
	}
	.weather-img img {
		max-height: 100%;
	}
	.temp {
		margin-top: 30px;
		font-size: 80px;
		line-height: 90px;
		font-weight: bold;
	}
	.feels-like {
		font-size: 15px;
		line-height: 17px;
		margin-top: 5px;
	}
	.right-block .top-block {
		display: flex;
		align-items: center;
		justify-content: space-between;
	}
	.time-zone {
		font-size: 24px;
		line-height: 30px;
		font-weight: bold;
	}
	.skeleton {
		background: #e2e2e2;
		color: #e2e2e2;
		overflow: hidden;
	}
</style>