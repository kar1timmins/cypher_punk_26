<script lang="ts">
  import CircuitMesh from './components/backgrounds/CircuitMesh.svelte';
  import Hero       from './components/sections/Hero.svelte';
  import About      from './components/sections/About.svelte';
  import Speakers   from './components/sections/Speakers.svelte';
  import Schedule   from './components/sections/Schedule.svelte';
  import Register   from './components/sections/Register.svelte';
</script>

<!--
  App.svelte — root composition layer.

  Layer order (z-index):
    0  — CircuitMesh  (fixed background)
    10 — page sections (relatively stacked, scroll normally)
-->

<!-- Fixed background: mesh grid + circuit SVG -->
<CircuitMesh />

<!-- Main scrollable content -->
<main class="relative z-10 flex flex-col">
  <Hero />
  <About />
  <Speakers />
  <Schedule />
  <Register />
</main>

<!-- Footer -->
<footer class="relative z-10 border-t border-white/5 px-4 py-8 text-center">
  <p class="font-mono text-xs text-slate-600">
    CYPHER/PUNK 2026 &nbsp;·&nbsp; all code is open-source &nbsp;·&nbsp;
    <span class="text-[#00F0FF]/40">0x{Math.floor(Math.random() * 0xFFFFFFFF).toString(16).toUpperCase().padStart(8, '0')}</span>
  </p>
</footer>
