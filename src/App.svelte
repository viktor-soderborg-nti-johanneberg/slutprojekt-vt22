<script>
  
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
    if (lastOption === undefined || lastOption.Name === "") {
      lastOption = options[Math.floor(Math.random() * options.length)]
      currentOption = options[Math.floor(Math.random() * options.length)]

      if (lastOption === currentOption) {
        generateOptions()
      } else if (currentOption.ParkingSpaces === undefined) {
        generateOptions()
      } else if (lastOption.ParkingSpaces === undefined) {
        generateOptions()
      } else if (lastOption.ParkingSpaces === currentOption.ParkingSpaces) {
        generateOptions()
      }
    } else {
      lastOption = currentOption
      currentOption = options[Math.floor(Math.random() * options.length)]

      if (currentOption === lastOption) {
        generateOptions()
      } else if (currentOption.ParkingSpaces === undefined) {
        generateOptions()
      } else if (lastOption.ParkingSpaces === currentOption.ParkingSpaces) {
        generateOptions()
      }
    }
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
    lastOption.Name = "";
    if (score > highscore) {
      highscore = score;
    }
    alert(`You lost! Your final score was ${score}! Close this to restart`);
    score = 0;
    generateOptions();
  }
  
  async function main() {
    let data = await loadData('http://parkering.linkoping.se/Parkeringsdata/ParkeringsdataV1.svc/GetParkeringsytaList/f7e5d2afb1dc41bb97e9118b592c0040/0');
    options = [...data];
    generateOptions();
  }
</script>

<main>
  {#await main()}
    <p>Waiting for data...</p>
    {:then}
      <section id="opt1">
        <h1>{lastOption.Name}</h1>
        <h2>{lastOption.ParkingSpaces}</h2>
        <h3>Highscore: {highscore}</h3>
      </section>
      <section id="opt2">
        <h1>{currentOption.Name}</h1>
        <button class="answer" on:click|preventDefault={() => checkAnswer('higher')}>Higher</button>
        <button class="answer" on:click|preventDefault={() => checkAnswer('lower')}>Lower</button>
        <h3>Score: {score}</h3>
      </section>
  {/await}
</main>

<style lang="scss">

  @import './variables';

  main {
    min-height: 100vh;
    height: 100%;
    min-width: 100vw;
    display: grid;
    grid-template-columns: repeat(2, 50vw);
    grid-template-areas: 
    'opt1 opt2';
    background-image: url("https://browsecat.net/sites/default/files/mountain-sunset-hd-2021-wallpapers-42341-48465-3645837.png");
    overflow: hidden;
    background-size: cover;
  }
  
  #opt1 {
    grid-area: opt1;
    @include center;
    border: 1px solid black;

    h3 {
      margin-left: $margin;
      margin-right: auto;
    }
    
    h2 {
      color: white;
    }
  }
  
  #opt2 {
    grid-area: opt2;
    @include center;
    border: 1px solid black;
    
    h3 {
      margin-right: $margin;
      margin-left: auto;
    }

    button {
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

  }

  #opt1 h3, #opt2 h3 {
    align-self: flex-end;
    margin-top: auto;
  }

  #opt1 h1, #opt2 h1 {
    margin-top: auto;
    text-align: center;
  }

  p, #opt1 h1, #opt2 h1, #opt1 h3, #opt2 h3 {
      color: white;
  }
</style>