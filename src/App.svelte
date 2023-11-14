<script lang="ts">
  import { Router, Route } from "svelte-routing";
  // import Register from "./routes/register/register.svelte";
  export let url = "";
</script>


<main>
  <Router {url}>
    <!-- <Route path="/"><Register /></Route>
    <Route path="/home"><Home /></Route> -->
  </Router>
</main>