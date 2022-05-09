<script>
  import { Router, Route, Link } from "svelte-navigator";
  import Home from './Home.svelte'
  import Game from './Game.svelte'
  import End from './End.svelte'
</script>

<Router>

  <main>
    <Route path="/">
      <Home />
    </Route>

    <Route path="/game">
      <Game />
    </Route>

    <Route path="/end">
      <End />
    </Route>

  </main>
</Router>

<style lang="scss">

  main {
    height: 100vh;
    width: 100vw;
    background-image: url("https://browsecat.net/sites/default/files/mountain-sunset-hd-2021-wallpapers-42341-48465-3645837.png");
    overflow: hidden;
    background-size: cover;
  }

</style>