<script lang="ts">
  // testing if AutoplayExample from my package still works 
  // update: it works

  // still very funny that im using a package i made
  import { Trace, TLine } from "trace-svelte";

  let step = 0;
  let highlights = [
    // TODO: make starting position be a variable to pass in? /refactor trace-svelte
    [], // need empty if you want none highlighted while "resting at step 1"
    [1,2],
    [2],
    [3]
  ];

  let is_playing = false;
  let play_interval : ReturnType<typeof setInterval>; 

  function increment() {
    if (step < highlights.length - 1) {
      step += 1;
    }
    else {
      togglePlaying();
      step = 0;
    }
  }

  function togglePlaying() { is_playing = !is_playing; }

  $: {
    if (!is_playing) { clearInterval(play_interval); }
    else {
      play_interval = setInterval(increment, 1250); 
    }
  }
</script>

<div class="description">
  <h3>Autoplay Example</h3>
  <button on:click={togglePlaying}>
    { is_playing ? "Stop" : "Play" }
  </button>
  <p>{step + 1} / {highlights.length}</p>
</div>
<p>
  Since controlling the step only requires changing the value, you can use functions to have an autoplay function.
</p>
<div class="trace">
  <Trace {step} {highlights}>
    <TLine>// print the following variable</TLine>
    <TLine>const name = "Louis";</TLine>
    <TLine>console.log(name);</TLine>
  </Trace>
</div> 


<style>
  .trace {
    width: 33%;
    margin: 0 auto;
    font-family: monospace;
    border: 2px solid black;
  }
  
  .description {
    display: inline-flex;
    margin: 0 auto;
    flex-direction: row;
    gap: 16px;
    align-items: center;
  }

  button {
    width: fit-content;
    height: fit-content;
    padding: 4px;
  }

  p { margin: 0 auto; }
</style>
