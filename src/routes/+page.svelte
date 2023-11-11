<script lang="ts">
  // still very funny that im using a package i made
  import { Trace, TLine } from "trace-svelte";

  // "universal"
  let step = 0;
  let current_page = 1;

  // PAGE 1: used to demonstrate basic function
  // value has to be template literal with each new line (ES6 way), with String.raw
  let page1_source = String.raw`const name = "Joey";
  console.log(name);
  // ta - da ~`;

  // to become an array of each line, split
  let page1_lines = page1_source.split(/\n/);
  

  let page1= [
    // [REFACTOR] (<Trace>): make starting position be a variable to pass in? 
    [], // need empty if you want none highlighted while "resting at step 1"
    [1,2],
    [2],
    [3]
  ];


  // PAGE 2: used to map PAGE 1 functionality with component/types
  let page2 = [
    [],
    [3],
    [2],
    [1]
  ];

  let is_playing = false;
  let play_interval : ReturnType<typeof setInterval>; 

  function increment() {
    if (step < page1.length - 1) {
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
    <h3 class="text-2xl">Camp Web MVP</h3>
    <p class="text-xl font-bold">current task: multi (Trace) "pages"</p>
    <p class="line-through text-green-600">sub task: read lines from string -> TLine</p>
    <p class="font-bold text-blue-600">sub task: make object with lines & highlights + component</p>
    <p class="text-neutral-500">sub task: implement levels of interactivity</p>
    <ul class="list-decimal text-neutral-500 pl-8">
      <li>no spotlight, just highlight, ping page with current step highlights</li>
      <li>complete codebase, with highlight</li>
      <li>"apple keynote-esque" add and subtract lines + highlight</li>
    </ul>
    <p class="font-bold">see read me for details</p>
  </div>


  <div class="flex gap-4 items-center">
    <button on:click={togglePlaying} class="px-4 py-2 border border-black rounded-xl">
      { is_playing ? "Stop" : "Play" }
    </button>
    <p>{step + 1} / {page1.length}</p>
  </div>


  <div class="flex gap-4">
    <label>
      <input type="radio" bind:group={current_page} value={1} />
      main.ts
    </label>

    <label>
      <input type="radio" bind:group={current_page} value={2} />
      test.ts
    </label>
  </div>


  <div class="p-8 border border-red-600">
    {#if current_page === 1}
      <div class="p-4 border border-blue-600">
        <Trace {step} highlights={page1}>
          {#each page1_lines as line}
            <TLine>{line}</TLine>
          {/each}
        </Trace>
      </div>
    {/if}


    {#if current_page === 2}
      <div class="p-4 border border-green-600">
        <Trace {step} highlights={page2}>
          <TLine>// backwards~</TLine>
          <TLine>const name = "Louis";</TLine>
          <TLine>console.log(name);</TLine>
        </Trace>
      </div>
    {/if}
  </div>
</main>
