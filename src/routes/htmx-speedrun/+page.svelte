<script lang="ts">
  import { onMount } from "svelte";
  import "@mux/mux-player";
  import type MuxPlayerElement from "@mux/mux-player";

  let video_player : MuxPlayerElement;
  let chapter_selector : HTMLSelectElement;

  // for mux video player
  let current_time = 0;
  let active_cue_point : CuePoint | undefined;

  type CuePoint = {
    time: number, // in seconds
    value: Chapter
  };

  type Chapter = {
    title: string,
    highlight: string
  };

  const cue_points : CuePoint[] = [
    {
      time: 0,
      value: {
        title: "Plan",
        highlight: "none"
      }
    },
    {
      time: 5,
      value: {
        title: "Halfway",
        highlight: "none"
      }
    }
  ];

  onMount(() => {
    video_player.addCuePoints(cue_points);
    video_player.addEventListener("cuepointchange", () => {
      active_cue_point = video_player.activeCuePoint;
    });
  });

  $: current_time = active_cue_point?.time ?? 0;
</script>

<main class="flex gap-8 w-full h-full min-w-screen min-h-screen p-8">
  <section class="flex flex-col basis-1/2">
    <a href="/" class="px-4 py-2 border rounded-full  w-fit">Back</a>
    <h3 class="text-2xl font-bold">HTMX Delete Speedrun DEMO</h3>
    <div class="flex gap-4 items-center">
      <p>Chapter: {active_cue_point?.value.title}</p>
      <select class="w-4 h-4" bind:value={active_cue_point} bind:this={chapter_selector}>
        {#each cue_points as cue}
          <option value={cue}>{cue.value.title}</option>
        {/each}
      </select>
    </div>
  </section>

  <section class="basis-1/2">
    <mux-player
      bind:this={video_player}
      currentTime={current_time}
      playback-id="3OErv8jicBruoWo8Af7PLkSOIZ7ue9PhVHdcIjzUWRI"
      metadata-video-id="video-id-54321"
      metadata-video-title="HTMX Delete Speedrun"
      metadata-viewer-user-id="user-id-123"
      stream-type="on-demand"
      accent-color="#FF0000"
    ></mux-player>
  </section>
</main>
