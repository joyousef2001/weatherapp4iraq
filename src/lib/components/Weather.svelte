<!-- src/Weather.svelte -->
<script>
  import { onMount } from "svelte";
  import { goto } from '$app/navigation'; // Import the goto function
  // import { link } from "svelte-routing";
  import { translations } from "../translations/translations.js";
  import Clock from "./Clock.svelte";
  export let weatherData = null;
  export let selectedCity = "Baghdad";
  export let cities = [];
  export let currentTime = "";
  export let sunriseTime = "";
  export let sunsetTime = "";
  export let WeatherWeak;
  // console.log(WeatherWeak)
  // Function to map temperature to color (similar to your Python function)
  function getTemperatureColor(temp) {
    const minTemp = -20; // Minimum temperature in the scale
    const maxTemp = 40; // Maximum temperature in the scale
    const normalizedTemp = (temp - minTemp) / (maxTemp - minTemp);
    const hue = 240 - 240 * normalizedTemp;
    const saturation = 80;
    const lightness = 60;
    return `hsl(${hue}, ${saturation}%, ${lightness}%)`;
  }

  $: tempColor = weatherData
    ? getTemperatureColor(weatherData.main.temp)
    : "#f0f0f0";

  function setWeatherIcon(weatherDescription) {
    const svgMapping = {
      "clear sky": "clear-day.svg",
      // 'clear sky': 'mist.svg',
      "few clouds": "partly-cloudy-day.svg",
      "scattered clouds": "partly-cloudy-day.svg",
      "broken clouds": "cloudy.svg",
      "shower rain": "rain.svg",
      rain: "rain.svg",
      thunderstorm: "thunderstorms.svg",
      snow: "snow.svg",
      mist: "mist.svg",
      // ... add more mappings for other weather descriptions
    };

    const defaultSvg = "not-available.svg"; // Default SVG image

    const svgFilename = svgMapping[weatherDescription] || defaultSvg;

    const svgSource = `./image/fill/svg/${svgFilename}`;

    return svgSource;
  }

  let scrollToContent = () => {
    let content = document.getElementById("content");
    content.scrollIntoView({ behavior: "smooth" });
  };

  function updateSelectedCity(city) {
    selectedCity = city;
  }

  function handleSearch() {
    // Perform search or other actions with selectedCity
    // For now, let's just log the selectedCity
    console.log("Selected city:", selectedCity);
  }
</script>

<!-- </div> -->
<div class="containier-sidebar">
  <header>
    <div class="card header">
      <!-- <div>
        <button on:click={scrollToContent}>شاهد الطقس</button>
      </div> -->
      <div class="search">
        <button on:click={() => goto('about')}>من نحن</button>
        
      </div>

      <div class="search">
        <button on:click={scrollToContent}>ابحث</button>
        <input
          type="text"
          placeholder="الكتب اسم المحافظة"
          bind:value={selectedCity}
        />
        {#if selectedCity && selectedCity.length > 0}
          <ul class="suggestions">
            {#each cities as city}
              {#if city.includes(selectedCity)}
                <!-- <li on:click={() => updateSelectedCity(city)}>{city}</li> -->
              {/if}
            {/each}
          </ul>
        {/if}
      </div>
    </div>
  </header>
  <div class="row">
    <div class="my-weather">
      <div class="left" id="content">
        <p class="iraq">
          العراق - {translations[selectedCity] || selectedCity}
        </p>
        {#if weatherData}
          <p class="text">درجة الحرارة:</p>
          <p class="text-value">{weatherData.main.temp} °C</p>
        {:else}
          <p>Loading weather data...</p>
        {/if}
      </div>

      <div class="right">
        {#if weatherData}
          <img
            src={setWeatherIcon(weatherData.weather[0].description)}
            alt=""
          />
        {:else}
          <p>Loading weather data...</p>
        {/if}
      </div>
    </div>
  </div>
  <!-- this card is have more information  -->
  <div class="card">
    {#if weatherData}
      <div class="weather-info">
        <p>
          الضغط الجوي: {weatherData.main.pressure}
        </p>
        <p>
          الرياح: {weatherData.wind.speed} متر/ث
        </p>
        <p>
          الرؤية: {weatherData.visibility} متر
        </p>
        <p>
          الرطوبة: {weatherData.main.humidity}%
        </p>
      </div>
      <div class="clock">
        <div>
          <div class="clocktime">
            <Clock
              start={true}
              stop={false}
              hour={currentTime.hour}
              minute={currentTime.minute}
              second={currentTime.second}
            />
          </div>
          <p class="current-time">الوقت الحالي للمدينة</p>
        </div>

        <div>
          <div class="clocktime">
            <Clock
              start={false}
              stop={true}
              hour={sunriseTime.hour}
              minute={sunriseTime.minute}
              second={sunriseTime.second}
            />
          </div>
          <p class="sunrise-time">شروق الشمس</p>
        </div>

        <div>
          <div class="clocktime">
            <Clock
              start={false}
              stop={true}
              hour={sunsetTime.hour}
              minute={sunsetTime.minute}
              second={sunsetTime.second}
            />
          </div>
          <p class="sunset-time">غروب الشمس</p>
        </div>
      </div>

      <div class="days">
        {#if WeatherWeak.length}
          {#each WeatherWeak as day, index}
            <!-- {console.log(day)} -->
            <!-- Debugging statement -->
            <div class="day">
              <img
                src={setWeatherIcon(weatherData.weather[0].description)}
                alt={day.date}
              />
              <p class="day-name">{day.arab_name}</p>
              <p class="day-date">{day.date}</p>
              <p class="day-temp">{day.temp} °C</p>
            </div>
            {#if index !== WeatherWeak.length - 1}
              <div class="day-divider" />
            {/if}
          {/each}
        {:else}
          <p>Loading weather data...</p>
        {/if}
      </div>
      <!-- </div> -->
    {:else}
      <p>Loading weather data...</p>
    {/if}
  </div>
  <!-- end this card is have mor information  -->

  <div class="footer card">
    <div class="icons">
      <div>
        <img class="image-footer" src="./image/webhazard.jpg" alt="" />
      </div>
      <div>
        <img class="image-footer" src="./image/code4iraq.jpg" alt="" />
      </div>
    </div>

    <div class="footer-end">جميع الحقوق الحفوظة لل code4iraq &copy; 2023</div>
  </div>
</div>

<style lang="scss">
  // .suggestions {
  //   display: none;
  //   list-style: none;
  //   padding: 0;
  //   margin: 0;
  //   position: absolute;
  //   background-color: white;
  //   border: 1px solid #ccc;
  //   border-radius: 5px;
  //   width: 100%;

  //   li {
  //     padding: 10px;
  //     cursor: pointer;
  //     &:hover {
  //       background-color: #f0f0f0;
  //     }
  //   }
  // }

  .footer {
    margin-top: 200px;
    margin-bottom: 50px;
    background-color: #333;
    color: #fff;
    text-align: center;
    padding: 20px;
    bottom: 0;
    .icons {
      width: 50%;
      display: flex;
      justify-content: space-around;
      align-items: center;
      margin: 100px 0;

      border-radius: 50%;
      img {
        width: 150px;
        height: 150px;
        padding: 10px;
        border: white solid 3px;
        border-radius: 50%;
      }
      & :hover {
        transition: all 1s;
        transform: scale(1.1);
      }
    }
    .footer-end {
      margin-top: 20px;
      padding-top: 10px;
      border-top: white solid 1px;
    }
  }

  .days {
    display: flex;
    justify-content: space-around;
    margin-top: 1rem;
    .day {
      text-align: center;
    }

    .day img {
      width: 75px;
      height: 75px;
    }

    .day-name {
      font-size: 24px;
      margin-top: 0.5rem;
    }
    .day-date {
      font-size: 0.9rem;
      color: #666;
    }

    .day-temp {
      font-size: 1.1rem;
      margin-top: 0.5rem;
    }

    .day-divider {
      width: 2px;
      // height: 100%;
      background-color: #ccc;
      // margin: 0 0.5rem;
    }
  }

  .clock {
    display: flex;
    justify-content: space-around;
    text-align: center;
    font-size: 1.2rem;
    margin-top: 20px;
    .clocktime {
      width: 200px;
    }
    .current-time {
      font-weight: bold;
    }

    .sunrise-time,
    .sunset-time {
      color: #666;
    }
  }

  .header {
    margin-top: 50px;
    display: flex;
    .search {
      display: flex;
      align-items: center;
      justify-content: space-between;
      margin-top: 20px;
      width: 40%;

      input[type="text"] {
        color: white;
        width: 60%; /* Adjusted width for input */
        flex: 1;
        padding: 10px;
        font-size: 16px;
        border: 1px solid #ccc;
        border-radius: 5px;
        outline: none;
        background-color: #333;
        transition: width 0.3s ease; /* Added transition */
      }
      input[type="text"]:hover {
        width: 70%; /* Increased width on hover */
      }

      button {
        border-radius: 5px;
        color: white;
        margin-left: 10px; /* Added margin for spacing */
        background-color: #333;
        padding: 10px;
        font-weight: bold;
      }
    }
  }

  /* Adjust styles for smaller screens */
  @media (max-width: 768px) {
    .search {
      flex-direction: column;
      align-items: flex-start;
    }
  }
  .weather-info {
    // flex: none;
    display: flex;
    justify-content: space-around;
    font-size: 24px;
    width: 100%;
  }

  .my-weather {
    display: flex;
    width: 100%;
    .right {
      padding: 20px;
      width: 30%;
      justify-content: start;
      display: flex;
      img {
        justify-content: center;
      }
    }
    .left {
      padding: 50px;
      .iraq {
        font-size: 48px;
        font-weight: bold;
      }
      .text {
        font-size: 18px;
      }
      .text-value {
        font-size: 32px;
        font-weight: bold;
        padding: 0px 100px;
      }
      width: 60%;
      justify-content: end;
    }
  }

  /* Media queries for responsiveness */
  @media (max-width: 768px) {
    .footer .icons {
      width: 100%;
      // flex-direction: column;
      align-items: center;
      margin: 1rem 0;
      img {
        width: 100px;
        height: 100px;
      }
    }
  }

  @media (max-width: 768px) {
    .footer {
      margin-top: 1rem;
    }
    .footer .icons {
      img {
        width: 75px;
        height: 75px;
      }
    }
  }

  @media (max-width: 768px) {
    .weather-info {
      // flex: none;
      font-size: 16px;
      flex-wrap: wrap; /* Allow items to wrap to the next row */
      width: 100%;
    }
  }
  @media (max-width: 768px) {
    .days {
      // flex-direction: column;
      flex-wrap: wrap; /* Allow items to wrap to the next row */
      align-items: center;
      .day {
        margin: 1rem 0;
      }
      // .day-divider {
      //   display: none;
      // }
    }
    .clock {
      // flex-direction: column;
      align-items: center;
      font-size: 16px;
      .clocktime {
        // width: 100%;
        width: 90px;
      }
    }
    .my-weather {
      // flex: none;
      // flex-direction: column;
      .right {
        width: 100%;
      }
      .left {
        width: 100%;
        padding: 1rem;
        .iraq {
          font-size: 24px;
        }
        .text {
          font-size: 16px;
        }
        .text-value {
          float: left;
          font-size: 18px;
          padding: 0;
        }
      }
    }
  }

  .containier-sidebar {
    width: 75%;
    margin-right: 350px;
  }
  @media (max-width: 768px) {
    .containier-sidebar {
      width: 100%;
      margin-right: 0;
    }
  }
</style>
