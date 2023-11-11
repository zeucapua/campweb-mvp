<script lang="ts">
  import "@mux/mux-player";
</script>

<main class="flex flex-col gap-8 w-full h-full min-w-screen min-h-screen p-8"> 
  <div>
    <h1 class="text-3xl font-bold">Camp Web MVP</h1>
    <p class="font-bold italic">see README for details</p>

    <section>
      <h2 class="text-xl font-bold text-green-600 line-through">task: multi (Trace) "pages"</h2>
      <p class="line-through text-green-600">sub task: read lines from string -> TLine</p>
      <p class="line-through text-green-600">sub task: make object with lines & highlights + component</p>
    </section>

    <section>
      <h2 class="text-xl font-bold line-through text-green-600">task: video player</h2>
      <p class="line-through text-green-600">sub task: use mux player</p>
      <p class="line-through text-green-600">sub task: add cue points + sync with variable</p>
      <p class="line-through text-green-600">sub task: seek video based on chapter</p>
    </section>

    <section>
      <h2 class="text-xl font-bold text-blue-500">task: mvp</h2>
      <p class="text-blue-500">sub task: cue point triggering step changes</p>
      <p class="text-neutral-500">sub task: ???</p>
    </section>

    <section>
      <h2 class="text-xl font-bold text-neutral-500">task: extra details</h2>
      <p class="text-neutral-500">sub task: video player details 
        (
          see 
          <a 
            href="https://x.com/bruno_simon/status/1722939286369005636?s=20"
            class="underline"
          >
            bruno simon's post
          </a>
        )
      </p>
      <p class="text-neutral-500">sub task: implement levels of interactivity</p>
      <ul class="list-decimal text-neutral-500 pl-8">
        <li>no spotlight, just highlight, ping page with current step highlights</li>
        <li>complete codebase, with highlight</li>
        <li>"apple keynote-esque" add and subtract lines + highlight</li>
      </ul>
      <p class="text-neutral-500">sub task: "admin" page</p>
      <ul class="list-decimal text-neutral-500 pl-8">
        <li>upload video/writing interactive source ("code pages")</li>
        <li>add chapter at current video time ("getCurrentTime"?)</li>
        <li>set chapter highlights
        (
          see
          <a href="https://rabbitstars.vercel.app/editor" class="underline">
            rabbitstars demo
          </a>
        ) 
        </li>
      </ul>
    </section>
  </div>

  <div class="flex flex-col gap-4">
    <h3 class="text-xl font-bold">Examples</h3>
    <div class="flex gap-2">
      <a href="/htmx-speedrun" class="px-4 py-2 rounded-xl border border-blue-600 bg-blue-600 text-white hover:bg-white hover:text-blue-600 transition-all duration-300">
        HTMX Delete Speedrun
      </a>
      <a href="/partykit" class="px-4 py-2 rounded-xl border border-red-600 bg-red-600 text-white hover:bg-white hover:text-red-600 transition-all duration-300">
        PartyKit
      </a>
    </div>
  </div>




</main>
