<script lang="ts">
  import { Router, Route } from "svelte-routing";
  import Register from "./routes/register.svelte";
  import Foodtype from "./routes/foodtype.svelte";
  import Home from "./routes/home.svelte";
  import Profil from "./routes/userProfil/profil.svelte";
  import Comments from "./routes/comments/comments.svelte";
  export let url = "";
</script>


<main>
  <Router {url}>

    <Route path="/"><Register /></Route>
    <Route path="/home"><Home/></Route>
    <Route path="/foodtype"><Foodtype/></Route>
    <Route path="/profil/:id"><Profil/></Route>
    <Route path="/comments"><Comments/></Route>

    <Route path="*"><p>404</p></Route>
  </Router> 
</main>
