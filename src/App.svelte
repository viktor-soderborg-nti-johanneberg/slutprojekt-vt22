<script>

  let score = 0;
  let highscore = 0;

  async function loadData(url) {
    try {      
      let resp = await fetch(url, {
        mode: 'no-cors'
      });
      
      console.log(resp)

      let data = await resp.json();
  
      console.log(data)
  
      return data
    } catch (error) {
      console.log(error) 
    }
  }

  loadData('http://parkering.linkoping.se/Parkeringsdata/ParkeringsdataV1.svc/GetParkeringsytaList/f7e5d2afb1dc41bb97e9118b592c0040/0');

</script>

<main>
  <section id="opt1">
    <h1>Google</h1> <!-- temp name -->
    <h2>2,000,000</h2>  <!-- temp numb -->
    <h3>Highscore: {highscore}</h3>
  </section>
  <section id="opt2">
    <button>Higher</button>
    <button>Lower</button>
    <h3>Score: {score}</h3>
  </section>
</main>

<style lang="scss">

  $color: red;

  main {
    min-height: 100vh;
    height: 100%;
    min-width: 100vw;
    display: grid;
    grid-template-areas: 
    'opt1 opt2';
  }

  #opt1 {
    border: 1px solid red;
    grid-area: opt1;
    display: flex;
    align-items: center;
  }

  #opt2 {
    border: 1px solid black;
    grid-area: opt2;
    display: flex;
    align-items: center;
    // background-image: url("http://api.higherlowergame.com/_client/images/general/builder.jpg");
    // overflow: hidden;
    // background-size: cover;
  }

  #opt1 h3 {
    margin-left: 1rem;
  }

  #opt2 h3 {
    margin-right: 1rem;
  }

  #opt1 h3, #opt2 h3 {
    align-self: flex-end;
  }

  #opt2 button {
    transition: all .5s ease;
    border: 3px solid white;
    color: yellow;
    padding: 15px 32px;
    text-align: center;
    text-decoration: none;
    font-size: 16px;
    margin: 0 auto;
    cursor: pointer;
    border-radius: 2rem;
    text-transform: uppercase;
    background-color : transparent;
  }

</style>