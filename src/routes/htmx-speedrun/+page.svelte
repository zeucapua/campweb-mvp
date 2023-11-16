<script lang="ts">
  import { onMount } from "svelte";
  import Codepage from "$lib/Codepage.svelte";

  import "@mux/mux-player";
  import type MuxPlayerElement from "@mux/mux-player";

  let video_player : MuxPlayerElement;
  let chapter_selector : HTMLSelectElement;

  // for Codepage's
  let current_page = "htmx-1"; // or 'htmx-2', refactor to dynamic page_id's

  // for mux video player
  let current_time = 0;
  let active_cue_point : CuePoint | undefined;

  // shape of cues for mux video player, value is JSON serializable
  type CuePoint = {
    time: number, // in seconds
    value: Chapter
  };

  type Chapter = {
    step: number, // for Codepage/Trace
    title: string,
    highlight: string[] // list of page_id's to highlight with current step
  };

  // to pass with mux video player
  const cue_points : CuePoint[] = [
    {
      time: 0,
      value: {
        step: 0,
        title: "Plan",
        highlight: ["htmx-1"]
      }
    },
    {
      time: 5,
      value: {
        step: 1,
        title: "Halfway",
        highlight: ["htmx-2"]
      }
    }
  ];

  // for Codepage
  const pages = [
    {
      id: "htmx-1",
      content: String.raw`app.delete("/deleteProject/:id", async (c) => {
        const project_id = c.req.param("id") as string;

        await db
          .delete(schema.projects)
          .where(
            eq(schema.projects.id, Number.parseInt(project_id))
          );
        
        return c.html(<div />);
      });`,
      highlights: [
        [2,3,4,5,6,7,8,9],
        [],
      ]
    },
    {
      id: "htmx-2",
      content: String.raw`export const ProjectCreator = () => {
        return (
          <div class="flex gap-4 p-8 border rounded-xl">
            <input 
              type="text"
              name="new_project"
              placeholder="Enter project name..."
              class="px-4 py-2 rounded-xl text-black"
            />
            <button
              type="button"
              hx-trigger="click"
              hx-post="/createProject"
              hx-include="[name='new_project']"
              hx-target="#punchers"
              hx-swap="beforeend"
              class="px-4 py-2 rounded-xl bg-white text-black"
            >
              Create
            </button>
          </div>
        );
      }`,
      highlights: [
        [],
        [12,13,14,15,16]
      ]
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

<main class="flex flex-col gap-8 w-full h-full min-w-screen min-h-screen p-8 bg-zinc-800">
  <div class="flex w-full h-full border border-2 border-zinc-700 rounded-xl">
    <section class="flex flex-col gap-8 basis-1/2 bg-zinc-700 rounded-l-lg p-8 text-white">
      <div class="flex flex-col gap-4">
        <a href="/" class="px-4 py-2 border rounded-full  w-fit">Back</a>
        <h3 class="text-2xl font-bold">HTMX Delete Speedrun DEMO</h3>
        <div class="flex gap-4 items-center">
          <p>Chapter: {active_cue_point?.value.title}</p>
          <select 
            class="w-4 h-4 rounded-lg text-black" 
            bind:value={active_cue_point} 
            bind:this={chapter_selector}
          >
            {#each cue_points as cue}
              <option value={cue}>{cue.value.title}</option>
            {/each}
          </select>
        </div>
      </div>

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

    <section class="basis-1/2 p-8 bg-white rounded-r-lg">
      <div class="flex flex-col">
        <div class="flex gap-4">
          <label>
            <input type="radio" bind:group={current_page} value="htmx-1" />
            index.tsx
          </label>

          <label>
            <input type="radio" bind:group={current_page} value="htmx-2" />
            components.tsx
          </label>
        </div>

        <div class="h-full max-h-96 font-mono overflow-y-auto">
        {#each pages as page}
            {#if page.id === current_page}
              <Codepage 
                step={active_cue_point?.value.step ?? 0} 
                highlights={page.highlights}
                code={page.content}
              /> 
            {/if}
          {/each}
        </div>
      </div>
    </section>
  </div>

  <p class="text-white text-center">demo by @zeu_dev {"<3"}</p>
</main>
