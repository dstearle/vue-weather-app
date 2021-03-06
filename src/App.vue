<template>

	<div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 70 ? 'warm' : ''">

		<main>

			<!-- Search Input -->
			<div class="search-box">

				<input 
					type="text" 
					class="search-bar" 
					placeholder="Search..."
					v-model="query"
					@keyup.enter="fetchWeather"
				/>

			</div>

			<!-- Information Wrapper -->
			<div class="weather-wrap" v-if="typeof weather.main != 'undefined'">

				<!-- Location & Date -->
				<div class="location-box">

					<div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>

					<div class="date">{{ dateBuilder() }}</div>

				</div>

				<!-- Temperature & Weather -->
				<div class="weather-box">

					<div class="temp">{{ Math.round(weather.main.temp) }}°F</div>

					<div class="weather">{{ weather.weather[0].main }}</div>

				</div>

			</div>

		</main>

	</div>

</template>

<script>

	export default {

		name: 'App',

		data() {

			return {

				api_key: '872e27b99ee063355a94208b358ae359',
				url_base: 'https://api.openweathermap.org/data/2.5/',
				query: '',
				weather: {}

			}

		},

		methods: {

			// Retrieves the data for weather
			fetchWeather() {

				// Fetches API information
				fetch(`${this.url_base}weather?q=${this.query}&units=imperial&APPID=${this.api_key}`)
				// Callback for json data
				.then(res => {
					return res.json();
				})
				// Passes json data to setResults method
				.then(this.setResults);

			},

			// Sets the data retrieved from fetchWeather to 'weather' in data
			setResults(results) {

				this.weather = results;

			},

			// Retrieves and formats the date for the UI
			dateBuilder () {

				// Retrieves today's date
				let d = new Date();
				
				// Array of strings for months and days to be used below
				let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
				let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];

				let day = days[d.getDay()];
				let date = d.getDate();
				let month = months[d.getMonth()];
				let year = d.getFullYear();

				// The formatted date to be used
				return `${day} ${date} ${month} ${year}`;

			}

		},

	}

</script>

<style>

	* {
		margin: 0;
		padding: 0;
		box-sizing: border-box;
	}

	body {
		font-family: 'montserrat', sans-serif;
	}

	#app {
		background-image: url('./assets/cold-bg.jpg');
		background-size: cover;
		background-position: bottom;
		transition: 0.4s;
	}

	#app.warm {
		background-image: url('./assets/warm-bg.jpg');
	}

	main {
		min-height: 100vh;
		padding: 25px;

		background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));
	}

	.search-box {
		width: 100%;
		margin-bottom: 30px;
	}

	.search-box .search-bar {
		display: block;
		width: 100%;
		padding: 15px;

		color: #313131;
		font-size: 20px;

		appearance: none;
		border: none;
		outline: none;
		background: none;

		box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
		background-color: rgba(255, 255, 255, 0.5);
		border-radius: 0px 16px 0px 16px;
		transition: 0.4s;
	}

	.search-box .search-bar:focus {
		box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
		background-color: rgba(255, 255, 255, 0.75);
		border-radius: 16px 0px 16px 0px;
	}

	.location-box .location {
		color: #FFF;
		font-size: 32px;
		font-weight: 500;
		text-align: center;
		text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
	}

	.location-box .date {
		color: #FFF;
		font-size: 20px;
		font-weight: 300;
		text-align: center;
		font-style: italic;
	}

	.weather-box {
		text-align: center;
	}

	.weather-box .temp {
		display: inline-block;
		padding: 10px 25px;
		color: #FFF;
		font-size: 102px;
		font-weight: 900;
		text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
		background-color: rgba(255, 255, 255, 0.25);
		border-radius: 16px;
		margin: 30px 0px;
		box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
	}

	.weather-box .weather {
		color: #FFF;
		font-size: 48px;
		font-weight: 700;
		font-style: italic;
		text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
	}
	
</style>