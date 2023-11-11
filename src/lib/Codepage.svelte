<script lang="ts">
  import { onMount } from "svelte";
  import { Trace, TLine } from "trace-svelte";

  export let code : string; 
  export let step : number;
  export let highlights : number[][];

  let lines : string[];

  onMount(() => {
    code = String.raw`${code}`
    lines = code.split(/\n/);
  });
</script>

{#if lines}
  <Trace {step} {highlights}> 
    {#each lines as line}
      <TLine>{line}</TLine>
    {/each}
  </Trace>
{:else}
  <p>Test</p>
{/if}
