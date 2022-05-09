<script>

  import { Router, Route, Link } from "svelte-navigator";

  let score = 0;
  let highscore = 0;
  let lastOption;
  let currentOption;
  let options = [];

  async function loadData(url) {
    try {      
      let resp = await fetch(url);

      let data = await resp.json();

      return data.ParkingAreaNewList
    } catch (error) {
      console.log(error) 
    }
  }
  
  function generateOptions() {
    if (lastOption == undefined) {
      lastOption = options[Math.floor(Math.random() * options.length)]
      currentOption = options[Math.floor(Math.random() * options.length)]

      if (lastOption === currentOption) {
        generateOptions()
      } else if (currentOption.ParkingSpaces === undefined) {
        generateOptions()
      } else if (lastOption.ParkingSpaces === undefined) {
        generateOptions()
      } else if (lastOption.ParkingSpaces === currentOption.ParkingSpaces) {
        generateOptions
      }
    } else {
      lastOption = currentOption
      currentOption = options[Math.floor(Math.random() * options.length)]

      if (lastOption === currentOption) {
        generateOptions()
      } else if (currentOption.ParkingSpaces === undefined) {
        generateOptions()
      } else if (lastOption.ParkingSpaces === undefined) {
        generateOptions()
      }
    }

    console.log(lastOption)
    console.log(currentOption)
  }
  
  function checkAnswer(answer) {
    if (answer == 'higher' && currentOption.ParkingSpaces > lastOption.ParkingSpaces) {
      score++
      generateOptions();
    } else if (answer == 'higher' && currentOption.ParkingSpaces < lastOption.ParkingSpaces) {
      gameOver();
    } else if (answer == 'lower' && currentOption.ParkingSpaces < lastOption.ParkingSpaces) {
      score++
      generateOptions();
    } else {
      gameOver();
    }
  }

  function gameOver() {
    if (score > highscore) {
      highscore = score;
    }
    score = 0;
    navigate('end')
  }

  async function main() {
    let data = await loadData('http://parkering.linkoping.se/Parkeringsdata/ParkeringsdataV1.svc/GetParkeringsytaList/f7e5d2afb1dc41bb97e9118b592c0040/0');
    options = [...data];
    generateOptions();
  }

</script>

<section>
  {#await main()}
    <p>Waiting for data...</p>
    {:then}
      <article id="opt1">
        <h1>{lastOption.Name}</h1>
        <h2>{lastOption.ParkingSpaces}</h2>
        <h3>Highscore: {highscore}</h3>
      </article>
      <article id="opt2">
        <h1>{currentOption.Name}</h1>
        <button on:click|preventDefault={() => checkAnswer('higher')}>Higher</button>
        <button on:click|preventDefault={() => checkAnswer('lower')}>Lower</button>
        <h3>Score: {score}</h3>
      </article>
  {/await}
</section>

<style lang="scss">

  $margin: 1rem;
  
  @mixin center {
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
    gap: 1rem;
  }

  section {
    min-height: 100vh;
    height: 100%;
    min-width: 100vw;
    display: grid;
    grid-template-columns: repeat(2, 50vw);
    grid-template-areas: 
    'opt1 opt2';
  }

  #opt1 {
    grid-area: opt1;
    @include center;
    border: 1px solid black;
  }
  
  #opt2 {
    grid-area: opt2;
    @include center;
    border: 1px solid black;
  }

  #opt1 h3 {
    margin-left: $margin;
    margin-right: auto;
  }

  #opt2 h3 {
    margin-right: $margin;
    margin-left: auto;
  }

  #opt1 h3, #opt2 h3 {
    align-self: flex-end;
    margin-top: auto;
    color: white;
  }

  #opt2 button {
    transition: all .5s ease;
    border: 3px solid white;
    color: yellow;
    padding: 15px 32px;
    text-align: center;
    text-decoration: none;
    font-size: 16px;
    cursor: pointer;
    border-radius: 2rem;
    text-transform: uppercase;
    background-color : transparent;
  }

  #opt1 h1, #opt2 h1 {
    margin-top: auto;
    color: white;
  }

  #opt1 h2, p {
    color: white;
  }

</style>