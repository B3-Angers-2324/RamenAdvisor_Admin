<script lang="ts">
  import { Router, Route } from "svelte-routing";
  import Register from "./routes/register.svelte";
  import Foodtype from "./routes/foodtype.svelte";
  import Home from "./routes/home.svelte";
  export let url = "";
</script>


<main>
  <Router {url}>

    <Route path="/"><Register /></Route>
    <Route path="/home"><Home/></Route>
    <Route path="/foodtype"><Foodtype/></Route>


    <Route path="*"><p>404</p></Route>
  </Router> 
</main>
