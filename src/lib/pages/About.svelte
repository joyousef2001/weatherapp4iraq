<!-- AboutUs.svelte -->

<script>
  import { link } from "svelte-routing";
</script>


<div class="section">
  <div class="search">
    <!-- <button on:click={scrollToContent}>من نحن</button> -->
  </div>
  <div class="about-content">
    <!-- <a href="{Link('Home')}">Back to Home</a> -->
    <!-- <a href="{link('Home')}">Back to Home</a> -->
    <h2>من نحن</h2>
    <p>نحن فريق مختص في توفير توقعات الطقس الدقFيقة والموثوقة...</p>
  </div>
</div>

<style>
  /* Add styles for the About Us section */
.about-content {
    padding: 20px;
    text-align: center;
  }
</style>
