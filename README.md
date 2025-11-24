<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Weather App</title>
  <style>
    body {
      font-family: "Segoe UI", Arial, sans-serif;
      background: linear-gradient(135deg, #0077b6, #00b4d8);
      color: #fff;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      height: 100vh;
      margin: 0;
    }

    h1 {
      margin-bottom: 20px;
      font-size: 2.2em;
      text-shadow: 1px 1px 3px rgba(0, 0, 0, 0.3);
    }

    .weather-box {
      background: rgba(255, 255, 255, 0.15);
      padding: 25px 40px;
      border-radius: 15px;
      box-shadow: 0 8px 20px rgba(0, 0, 0, 0.2);
      text-align: center;
      width: 320px;
    }

    input {
      width: 70%;
      padding: 10px;
      border: none;
      border-radius: 5px;
      outline: none;
      font-size: 1em;
    }

    button {
      background: #fff;
      color: #0077b6;
      border: none;
      border-radius: 5px;
      padding: 10px 15px;
      margin-left: 5px;
      font-size: 1em;
      font-weight: bold;
      cursor: pointer;
      transition: background 0.3s, transform 0.2s;
    }

    button:hover {
      background: #00b4d8;
      color: #fff;
      transform: scale(1.05);
    }

    .weather-info {
      margin-top: 20px;
      font-size: 1.1em;
      line-height: 1.5em;
    }

    .temp {
      font-size: 2em;
      font-weight: bold;
      margin-top: 10px;
    }

    .icon {
      width: 80px;
      height: 80px;
    }

    .location {
      font-weight: bold;
      font-size: 1.2em;
    }

    .error {
      color: #ffdddd;
      margin-top: 10px;
      font-size: 0.9em;
    }
  </style>
</head>
<body>

  <h1>🌤 Weather App</h1>

  <div class="weather-box">
    <div>
      <input type="text" id="cityInput" placeholder="Enter city name" />
      <button onclick="getWeather()">Search</button>
      <button onclick="getLocationWeather()">📍 My Location</button>
    </div>

    <div class="weather-info" id="weatherInfo">
      <p>Search for a city to see weather details.</p>
    </div>
  </div>

  <script>
    const apiKey = "YOUR_API_KEY_HERE";
    const weatherInfo = document.getElementById("weatherInfo");

    // Fetch weather by city name
    async function getWeather() {
      const city = document.getElementById("cityInput").value;
      if (!city) {
        weatherInfo.innerHTML = "<p class='error'>⚠ Please enter a city name.</p>";
        return;
      }

      const url = https://api.openweathermap.org/data/2.5/weather?q=${city}&units=metric&appid=${apiKey};
      await fetchWeatherData(url);
    }

    // Fetch weather by user's current location
    function getLocationWeather() {
      if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(success => {
          const lat = success.coords.latitude;
          const lon = success.coords.longitude;
          const url = https://api.openweathermap.org/data/2.5/weather?lat=${lat}&lon=${lon}&units=metric&appid=${apiKey};
          fetchWeatherData(url);
        }, () => {
          weatherInfo.innerHTML = "<p class='error'>⚠ Unable to access location.</p>";
        });
      } else {
        weatherInfo.innerHTML = "<p class='error'>⚠ Geolocation not supported.</p>";
      }
    }

    // Fetch and display weather data
    async function fetchWeatherData(url) {
      try {
        const response = await fetch(url);
        if (!response.ok) throw new Error("City not found");

        const data = await response.json();
        displayWeather(data);
      } catch (error) {
        weatherInfo.innerHTML = <p class='error'>❌ ${error.message}</p>;
      }
    }

    // Display data
    function displayWeather(data) {
      const { name, main, weather, sys } = data;
      const icon = https://openweathermap.org/img/wn/${weather[0].icon}@2x.png;

      weatherInfo.innerHTML = `
        <div class="location">${name}, ${sys.country}</div>
        <img src="${icon}" alt="Weather Icon" class="icon">
        <div class="temp">${Math.round(main.temp)}°C</div>
        <div>${weather[0].main} - ${weather[0].description}</div>
        <div>Humidity: ${main.humidity}%</div>
        <div>Pressure: ${main.pressure} hPa</div>
      `;
    }
  </script>

</body>
</html>
