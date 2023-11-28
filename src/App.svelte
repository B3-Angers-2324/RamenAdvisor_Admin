<script lang="ts">
  import { Router, Route } from "svelte-routing";
  import Register from "./routes/register.svelte";
  import Foodtype from "./routes/foodtype.svelte";
  import Home from "./routes/home.svelte";
  import Profil from "./routes/ownerProfil/profil.svelte";
  import Comments from "./routes/comments/comments.svelte";
  import BanUser from "./routes/banUser/banUser.svelte";
  import BanUserId from "./routes/banUser/user.svelte";
  import ListOwner from "./routes/ownerList/ownerList.svelte";
  import ListOwnerProfil from "./routes/ownerList/owner.svelte";
  export let url = "";
</script>


<main>
  <Router {url}>

    <Route path="/"><Register /></Route>
    <Route path="/home"><Home/></Route>
    <Route path="/foodtype"><Foodtype/></Route>
    <Route path="/profil/:id"><Profil/></Route>
    <Route path="/comments"><Comments/></Route>
    <Route path="/banUser/home"><BanUser/></Route>
    <Route path="/banUser/user/:id"><BanUserId/></Route>
    <Route path="/ownerList/home"><ListOwner/></Route>
    <Route path="/ownerList/owner/:id"><ListOwnerProfil/></Route>

    <Route path="*"><p>404</p></Route>
  </Router> 
</main>
