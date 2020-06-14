<template>
	<main :style="{ backgroundColor: temperatureColor }">
		<div id="app">
			<div class="search-box">
				<form @submit.prevent="fetchWeather">
					<input
						v-model="query"
						type="text"
						class="search-bar"
						placeholder="Search..."
					/>
				</form>
			</div>

			<div v-if="typeof weather.main != 'undefined'" class="weather-wrap">
				<div class="location-box">
					<div class="location">{{ weather.name }}</div>
					<div class="date">Monday 20 April 2020</div>
				</div>

				<div class="weather-box">
					<div class="temp">{{ Math.round(weather.main.temp) }}°C</div>
					<div class="weather">{{ weather.weather[0].main }}</div>
				</div>
			</div>

			<div v-if="notification_show" class="notification-box">
				<div v-for="notification in notifications" :key="notification.id">
					<p>{{ notification[i].message }}</p>
				</div>
			</div>
		</div>
	</main>
</template>

<script>
import axios from 'axios'
export default {
	name: 'App',
	data() {
		return {
			api_key: 'f348f3dae7dc72d5fb2c3e12306f80f5',
			base_url: 'https://api.openweathermap.org/data/2.5/',
			query: '',
			weather: {},
			temperature: null,
			notification_show: false,
			notifications: [],
			i: -1
		}
	},
	computed: {
		temperatureColor() {
			switch (true) {
				case this.temperature == null:
					return 'white'
				case this.temperature < 5:
					return 'blue'
				case this.temperature < 10:
					return 'lightblue'
				case this.temperature < 17:
					return '#ffe27a'
				case this.temperature < 24:
					return 'orange'
				case this.temperature < 26:
					return 'orangered'
				case this.temperature < 100:
					return 'darkred'

				default:
					return 'white'
			}
		}
	},

	methods: {
		fetchWeather() {
			axios
				.get(
					`${this.base_url}weather?q=${this.query}&units=metric&appid=${this.api_key}`
				)
				.then((response) => {
					this.weather = response.data
					this.temperature = Math.round(this.weather.main.temp)
				})
				.catch((error) => {
					const notification = {
						type: 'error',
						message: "We can't find this city : " + error.message
					}
					this.notifications.push({
						...notification
					})
					this.i++
					this.notification = true
					throw error
				})
		}
	}
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

main {
	height: 100vh;
	padding: 25px;
	transition: 0.4s;
}

.search-box {
	width: 100%;
	margin-bottom: 30px;
}

.search-bar {
	display: block;
	width: 100%;
	padding: 15px;
	color: #272643;
	font-size: 20px;
	appearance: none;
	border: none;
	outline: none;
	background: none;
	box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
	background-color: rgba(255, 255, 255, 0.5);
	border-radius: 0px 16px;
	transition: 0.4s;
}

.search-bar:focus {
	box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
	background-color: rgba(255, 255, 255, 0.85);
	border-radius: 16px 0px;
}

.location-box .location {
	color: #fff;
	font-size: 32px;
	font-weight: 500;
	text-align: center;
	text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

.location-box .date {
	color: #fff;
	font-size: 20px;
	font-weight: 300;
	font-style: italic;
	text-align: center;
}

.weather-box {
	text-align: center;
}

.weather-box .temp {
	display: inline-block;
	padding: 10px 25px;
	color: #fff;
	font-size: 102px;
	font-weight: 900;
	text-shadow: 3px 6px rgba(0, 0, 0, 0, 0.25);
	background-color: rgba(255, 255, 255, 0.25);
	border-radius: 16px;
	margin: 30px 0px;
	box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
	transition: 0.2s;
}

.weather-box .temp:active {
	transform: translate(3px, 6px);
	box-shadow: 0px 0px rgba(0, 0, 0, 0.25);
	transition: 0.2s;
}

.weather-box .weather {
	color: #fff;
	font-size: 48px;
	font-weight: 700;
	font-style: italic;
	text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}
</style>
