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
	let humidity
	let speed
	let visibility
	let sunrise
	let sunset

	navigator.geolocation.getCurrentPosition((position) => {
		const lat  = position.coords.latitude;
		const lon = position.coords.longitude;
		coords = `${lat}, ${lon}`
		getWeather()
	})

	function timeConverter(UNIX_timestamp){
		let a = new Date(UNIX_timestamp * 1000)
		let hour = a.getHours()
		let min = "0" + a.getMinutes()
		let time = hour + ':' + min.substr(-2)
		return time;
	}

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
					humidity = data.main.humidity
					speed = data.wind.speed
					visibility = (data.visibility / 1000)
					sunrise = timeConverter(data.sys.sunrise)
					sunset = timeConverter(data.sys.sunset)

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
				<form class="form-block" on:submit|preventDefault={getWeather} class:skeleton={skeleton}>
					<input type="text" name="coords" bind:value={coords} class="coords">
				</form>
			</div>
			<div class="weather-block" class:skeleton={skeleton}>
				<div class="weather-item">
					<div class="caption">Влажность</div>
					<div class="info-block">
						<div class="img-block">
							<img src="icons/humidity.png" alt="img">
						</div>
						<div class="text-block">{humidity}%</div>
					</div>
				</div>
				<div class="weather-item" class:skeleton={skeleton}>
					<div class="caption">Скорость ветра</div>
					<div class="info-block">
						<div class="img-block">
							<img src="icons/wind.png" alt="img">
						</div>
						<div class="text-block">{speed}</div>
					</div>
				</div>
				<div class="weather-item" class:skeleton={skeleton}>
					<div class="caption">Направление ветра</div>
					<div class="info-block">
						<div class="img-block">
							<img src="icons/compass.png" alt="img">
						</div>
						<div class="text-block">N</div>
					</div>
				</div>
				<div class="weather-item" class:skeleton={skeleton}>
					<div class="caption">Видимость</div>
					<div class="info-block">
						<div class="img-block">
							<img src="icons/binocular.png" alt="img">
						</div>
						<div class="text-block">{visibility} км</div>
					</div>
				</div>
				<div class="weather-item" class:skeleton={skeleton}>
					<div class="caption">Восход</div>
					<div class="info-block">
						<div class="img-block">
							<img src="icons/sunrise.png" alt="img">
						</div>
						<div class="text-block">{sunrise}</div>
					</div>
				</div>
				<div class="weather-item" class:skeleton={skeleton}>
					<div class="caption">Закат солнца</div>
					<div class="info-block">
						<div class="img-block">
							<img src="icons/sunrise.png" alt="img">
						</div>
						<div class="text-block">{sunset}</div>
					</div>
				</div>
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
		padding: 0 15px;
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
		overflow: hidden;
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
		position: relative;
	}
	.skeleton:after {
		content: '';
		background: #e2e2e2;
		position: absolute;
		top: 0;
		right: 0;
		bottom: 0;
		left: 0;
		z-index: 10;
	}
	.coords {
		height: 40px;
		font-size: 18px;
		color: #303030;
		text-align: right;
		padding: 0 10px;
		border: none;
		border-radius: 10px;
		outline: none;
	}
	.weather-block {
		display: grid;
		grid-template-columns: 1fr 1fr 1fr;
		grid-gap: 20px;
		gap: 20px;
		margin-top: 20px;
	}
	.weather-item {
		background: hsla(0,0%,100%,.95);
		padding: 20px;
		border-radius: 20px;
		overflow: hidden;
	}
	.weather-item .caption {
		text-align: right;
	}
	.weather-item .info-block {
		display: flex;
		justify-content: space-between;
		margin-top: 10px;
	}
	.weather-item .info-block .img-block {
		line-height: 0;
		height: 100px;
		width: 100px;
	}
	.weather-item .info-block .img-block img {
		max-width: 100%;
	}
	.weather-item .info-block .text-block {
		font-size: 32px;
		line-height: 35px;
		font-weight: bold;
	}

	@media (max-width: 1200px) {
		.weather-item .info-block .img-block {
			line-height: 0;
			height: 50px;
			width: 50px;
		}
		.weather-item .info-block {
			align-items: center;
		}
	}
	@media (max-width: 992px) {
		.main-block {
			display: block;
			max-width: 600px;
		}
		.left-block {
			width: 100%;
		}
		.right-block {
			width: 100%;
		}
		.wrapper {
			padding: 15px;
		}
		.form-block {
			display: none;
		}
		.weather-block {
			grid-template-columns: 1fr 1fr;
		}
	}
	@media (max-width: 500px) {
		.weather-block {
			grid-template-columns: 1fr;
		}
		.weather-img {
			height: 150px;
		}
		.temp {
			margin-top: 10px;
			font-size: 60px;
			line-height: 70px;
		}
	}
</style>