<!-- src/App.svelte -->

<script>
  import { onMount } from "svelte";
  import Weather from "$lib/components/Weather.svelte";
  import Sidebar from "$lib/layouts/Sidebar.svelte";
  let selectedCity = "Baghdad";
  let currentTime = "";
  let sunriseTime = "";
  let sunsetTime = "";
  let weatherData = null;
  // let temperatureData;
  let cities = [
    "Baghdad",
    "Basra",
    "Mosul",
    "Erbil",
    "Kirkuk",
    "Najaf",
    "Sulaymaniyah",
    "Duhok",
    "Hilla",
    "Karbala",
    "Nasiriyah",
    "Amara",
    "Ramadi",
    "Fallujah",
    "Diwaniyah",
    "Kut",
    "Baqubah",
    "Sinjar",
    "Tikrit",
    "Samarra",
    // "Rutba",
    // "Ranya",
    "Zakho",
    // "Makhmour",
    // "Khanaqin",
    // "Zubair",
    "Shingal",
    "Halabja",
    "Rania",
    "Rawah",
    "Hit",
  ];

  let YOUR_API_KEY = "9934c71e10d175de74419fee0d31ecb5";
  async function getWeatherData() {
    try {
      const response = await fetch(
        `https://api.openweathermap.org/data/2.5/weather?q=${selectedCity},Iraq&appid=${YOUR_API_KEY}&units=metric`
      );
      if (response.ok) {
        weatherData = await response.json();
      } else {
        console.error("Error fetching weather data:", response.statusText);
      }
    } catch (error) {
      console.error("Error fetching weather data:", error);
    }
  }
  async function fetchWeatherData() {
    try {
      const response = await fetch(
        `https://api.openweathermap.org/data/2.5/weather?q=${selectedCity},Iraq&appid=${YOUR_API_KEY}&units=metric`
      );
      const data = await response.json();
      weatherData = data;

      updateCurrentTime();
      // currentTime = formatTime(weatherData?.dt, weatherData?.timezone);
      sunriseTime = formatTime(weatherData?.sys.sunrise, weatherData?.timezone);
      sunsetTime = formatTime(weatherData?.sys.sunset, weatherData?.timezone);
    } catch (error) {
      console.error("Error fetching weather data:", error);
    }
  }
  // let weatherForecast = []; // Declare the weatherForecast variable
  async function getWeatherForecast() {
    try {
      const response = await fetch(
        `https://api.openweathermap.org/data/2.5/forecast?q=${selectedCity},Iraq&appid=${YOUR_API_KEY}&units=metric`
      );
      if (response.ok) {
        const data = await response.json();
        return data.list; // Assuming data.list contains the forecast entries
      } else {
        console.error(
          "Error fetching weather forecast data:",
          response.statusText
        );
      }
    } catch (error) {
      console.error("Error fetching weather forecast data:", error);
    }
  }
  function findHighestTemperaturesByDay(forecastData) {
    const highestTempsByDay = {};

    for (const entry of forecastData) {
      const date = new Date(entry.dt_txt).toLocaleDateString();
      const temp = entry.main.temp;

      if (!(date in highestTempsByDay) || temp > highestTempsByDay[date]) {
        highestTempsByDay[date] = { temp: temp, weatherData: entry };
      }
    }

    return highestTempsByDay;
  }
  const dayNameMapping = {
    Sun: "الأحد",
    Mon: "الإثنين",
    Tue: "الثلاثاء",
    Wed: "الأربعاء",
    Thu: "الخميس",
    Fri: "الجمعة",
    Sat: "السبت",
  };

  // Define the function to convert English day names to Arabic
  function convertToArabicDayName(englishDayName) {
    // Add your day name mapping logic here
    return dayNameMapping[englishDayName];
  }

  // the day and date
  let WeatherWeak = [];
  // Define the fetchAndFindHighestTemperatures function
  async function fetchAndFindHighestTemperatures() {
    try {
      WeatherWeak = [];
      const forecastData = await getWeatherForecast();
      const highestTemperatures = await findHighestTemperaturesByDay(
        forecastData
      );
      // console.log(highestTemperatures);

      let temperatureData = highestTemperatures;
      // console.log(temperatureData);

      if (temperatureData) {
        // Now that temperatureData is ready, you can use it
        // console.log(temperatureData);

        // Convert and print the data with Arabic day names
        for (const date in temperatureData) {
          const [month, day, year] = date.split("/");
          const jsDate = new Date(`${year}-${month}-${day}`);
          const arabicDayName = convertToArabicDayName(
            jsDate.toLocaleDateString("en", { weekday: "short" })
          );
          WeatherWeak.push({
            arab_name: arabicDayName,
            date: date,
            temp: temperatureData[date]["temp"],
            weatherData: temperatureData[date]["weatherData"],
          });
          // console.log(`${arabicDayName} - ${date}: ${temperatureData[date]}`);
        }
      } else {
        // Handle the case where temperatureData is not available
        console.log("Temperature data is not available");
      }
      // console.log(WeatherWeak)
    } catch (error) {
      console.error("Error fetching and processing data:", error);
    }
  }

  // Call the fetchAndFindHighestTemperatures function
  fetchAndFindHighestTemperatures();

  // Call the function to fetch and find highest temperatures

  // Call the function to fetch weather forecast data

  getWeatherData(); // Fetch weather data when the component mounts
  function formatTime(timestamp, originalTimezoneOffset) {
    const userTimezoneOffset = new Date().getTimezoneOffset();
    const utcDate = new Date((timestamp + originalTimezoneOffset) * 1000);
    const userDate = new Date(
      utcDate.getTime() + (userTimezoneOffset - originalTimezoneOffset) * 60000
    );

    return {
      hour: userDate.getHours(),
      minute: userDate.getMinutes(),
      second: userDate.getSeconds(),
    };
  }

  function updateCurrentTime() {
    currentTime = formatTime(weatherData?.dt, weatherData?.timezone);
  }

  function handleCitySelect(event, city) {
    selectedCity = city;
    getWeatherData();
    fetchWeatherData();
    fetchAndFindHighestTemperatures();
    console.log(WeatherWeak);
  }

  // Update the current time every second
  setInterval(updateCurrentTime, 1000);
  onMount(fetchWeatherData);
  // import { Link } from 'svelte-routing';
  // console.log(WeatherWeak)
</script>

<main>
  <!-- <Link to="/about-us">من نحن</Link> -->
  <!-- <style lang="scss" src="./Weather.scss">
  </style> -->

  <div class="container">
    <!-- <div style="width: 100%;"> -->
    <Sidebar {cities} {selectedCity} {handleCitySelect} />
    <Weather
      {cities}
      {weatherData}
      {selectedCity}
      {currentTime}
      {sunriseTime}
      {sunsetTime}
      {WeatherWeak}
    />
  </div>
</main>

<style>
</style>
