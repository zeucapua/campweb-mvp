<script lang="ts">
  // still very funny that im using a package i made
  import { Trace, TLine } from "trace-svelte";

  let step = 0;
  let highlights = [
    // [REFACTOR] (<Trace>): make starting position be a variable to pass in? 
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

<main class="flex flex-col gap-8 w-full h-full min-w-screen min-h-screen p-8"> 
  <div>
    <h3 class="text-xl">Camp Web MVP</h3>
    <p>current task: multi (Trace) "pages"</p>
    <p>see read me for details</p>
  </div>

  <div class="flex gap-4 items-center">
    <button on:click={togglePlaying} class="px-4 py-2 border rounded-xl">
      { is_playing ? "Stop" : "Play" }
    </button>
    <p>{step + 1} / {highlights.length}</p>
  </div>

  <Trace {step} {highlights}>
    <TLine>// print the following variable</TLine>
    <TLine>const name = "Louis";</TLine>
    <TLine>console.log(name);</TLine>
  </Trace>
</main>
