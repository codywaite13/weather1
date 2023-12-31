# weather1
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Weather App</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      text-align: center;
    }
    #weather-container {
      margin-top: 50px;
    }
  </style>
</head>
<body>
  <h1>Weather App</h1>

  <label for="city">Enter city:</label>
  <input type="text" id="city" placeholder="Enter city">
  <button onclick="getWeather()">Get Weather</button>

  <div id="weather-container"></div>

  <script>
    function getWeather() {
      const apiKey = 'YOUR_OPENWEATHERMAP_API_KEY';
      const city = document.getElementById('city').value;

      if (!city) {
        alert('Please enter a city.');
        return;
      }

      const apiUrl = `https://api.openweathermap.org/data/2.5/onecall?q=${city}&appid=${apiKey}&units=metric`;

      fetch(apiUrl)
        .then(response => response.json())
        .then(data => {
          displayWeather(data);
        })
        .catch(error => {
          console.error('Error fetching weather data:', error);
        });
    }

    function displayWeather(data) {
      const weatherContainer = document.getElementById('weather-container');
      weatherContainer.innerHTML = '';

      const cityName = document.createElement('h2');
      cityName.textContent = `Weather in ${data.timezone}`;
      weatherContainer.appendChild(cityName);

      const currentTemperature = document.createElement('p');
      currentTemperature.textContent = `Current Temperature: ${data.current.temp} °C`;
      weatherContainer.appendChild(currentTemperature);

      const hourlyForecastTitle = document.createElement('h3');
      hourlyForecastTitle.textContent = 'Hourly Forecast';
      weatherContainer.appendChild(hourlyForecastTitle);

      const hourlyForecastList = document.createElement('ul');
      data.hourly.slice(0, 5).forEach(hourly => {
        const listItem = document.createElement('li');
        listItem.textContent = `${new Date(hourly.dt * 1000).toLocaleTimeString()}: ${hourly.temp} °C`;
        hourlyForecastList.appendChild(listItem);
      });
      weatherContainer.appendChild(hourlyForecastList);

      const hailTracingInfo = document.createElement('p');
      hailTracingInfo.textContent = 'Hail tracing information can go here based on the detailed data provided by the API.';
      weatherContainer.appendChild(hailTracingInfo);
    }
  </script>
</body>
</html>
