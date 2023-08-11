<script>
  import { onMount, onDestroy } from "svelte";

  export let current = false; // Whether to show current time
  export let start = false;
  export let stop = false;
  export let hour;
  export let minute;
  export let second;
  export let darkMode = true;

  let interval;

  function updateClock() {
    const now = new Date();
    hour = now.getHours();
    minute = now.getMinutes();
    second = now.getSeconds();
  }

  onMount(() => {
    console.log("mounted the clock");
    if (current) {
      interval = setInterval(updateClock, 1000);
    }
  });

  onDestroy(() => {
    console.log("destroyed the clock");
    clearInterval(interval);
  });

  let interval1;

  // Function to update the clock based on start and stop values
  // function updateClockTimer() {
   
  //   second1 += 1;
  //   if (second1 === 60) {
  //     second1 = 0;
  //     minute1 += 1;
  //     if (minute1 === 60) {
  //       minute1 = 0;
  //       hour1 += 1;
  //       if (hour1 === 24) {
  //         hour1 = 0;
  //       }
  //     }

  //     hour=hour1;
  //     minute=minute1;
  //     second=second1;
  //   }

    // console.log("updateClockTimer hour:", hour, "minute:", minute, "second:", second);
  // }

  // Watch for changes in start and stop values
  $: {
    if (start) {
      // interval1 = setInterval(updateClockTimer, 1000);
      
    } else if (stop) {
      // clearInterval(interval1);
    }
  }
</script>


<svg viewBox="-50 -50 100 100" class:dark-mode={darkMode}>
  <circle class="clock-face" r="48" />

  <!-- markers -->
  {#each [0, 5, 10, 15, 20, 25, 30, 35, 40, 45, 50, 55] as minute}
    <line class="major" y1="35" y2="45" transform="rotate({30 * minute})" />

    {#each [1, 2, 3, 4] as offset}
      <line
        class="minor"
        y1="42"
        y2="45"
        transform="rotate({6 * (minute + offset)})"
      />
    {/each}
  {/each}

  <!-- hour hand -->
  <line
    class="hour"
    y1="2"
    y2="-20"
    transform="rotate({30 * hour + minute / 2})"
  />

  <!-- minute hand -->
  <line
    class="minute"
    y1="4"
    y2="-30"
    transform="rotate({6 * minute + second / 10})"
  />

  <!-- second hand -->
  <g transform="rotate({6 * second})">
    <line class="second" y1="10" y2="-38" />
    <line class="second-counterweight" y1="10" y2="2" />
  </g>
</svg>


<style>
  /* Apply styles based on darkMode prop */
  .clock-face {
    stroke: #333;
    fill: white;
  }

  .minor {
    stroke: #999;
    stroke-width: 0.5;
  }

  .major {
    stroke: #333;
    stroke-width: 1;
  }

  .hour {
    stroke: #333;
  }

  .minute {
    stroke: #666;
  }

  .second,
  .second-counterweight {
    stroke: rgb(180, 0, 0);
  }

  .second-counterweight {
    stroke-width: 3;
  }

  /* Override styles for dark mode */
  .dark-mode .clock-face {
    stroke: #ccc;
    fill: #111;
  }

  .dark-mode .minor {
    stroke: #666;
  }

  .dark-mode .major {
    stroke: #ccc;
  }

  .dark-mode .hour {
    stroke: #ccc;
  }

  .dark-mode .minute {
    stroke: #aaa;
  }

  .dark-mode .second,
  .dark-mode .second-counterweight {
    stroke: rgb(255, 60, 60);
  }
  
</style>

