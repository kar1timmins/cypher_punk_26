<script lang="ts">
  import GlassCard from '../ui/GlassCard.svelte';
  import DecryptText from '../ui/DecryptText.svelte';

  const HACKATHON_DATE = '// 2026.08.08 — 2026.08.16';
  const LOCATION      = 'LOCATION: Baseline Community & Fund - Dublin, Ireland';
  const THEME         = 'THEME: Zero Knowledge for Social Impact';
  const TAGLINE       = 'BREAK THE CIPHER. BUILD THE FUTURE.';

  function scrollToRegister(e: MouseEvent) {
    e.preventDefault();
    document.getElementById('register')?.scrollIntoView({ behavior: 'smooth' });
  }
</script>

<section
  id="hero"
  class="relative z-10 flex min-h-screen items-center justify-center px-4 py-24"
>
  <GlassCard glow="cyan" class="w-full max-w-3xl text-center">
    <!-- Top label bar -->
    <div class="mb-6 flex items-center justify-center gap-3">
      <span class="h-px w-12 bg-[#00F0FF] opacity-60"></span>
      <span class="font-mono text-xs tracking-[0.3em] text-[#00F0FF] opacity-70 uppercase">
        Zero Knowledge Hackathon
      </span>
      <span class="h-px w-12 bg-[#00F0FF] opacity-60"></span>
    </div>

    <!-- Main title with decrypt mechanic -->
    <h1 class="mb-4 text-5xl font-bold leading-tight md:text-7xl flex items-baseline justify-center">
      <DecryptText
        text="CYPHER"
        tag="span"
        duration={500}
        steps={16}
        class="glow-cyan"
      /><span class="text-white/20">/</span><DecryptText
        text="PUNK"
        tag="span"
        duration={500}
        steps={16}
        class="glow-magenta"
      />
    </h1>

    <!-- Subtitle -->
    <p class="mb-2 font-mono text-sm tracking-widest text-slate-400">
      <DecryptText text={HACKATHON_DATE} tag="span" duration={350} steps={10} />
    </p>
    <p class="mb-8 font-mono text-xs tracking-[0.25em] text-slate-500 uppercase">
      <DecryptText text={LOCATION} tag="span" duration={300} steps={8} />
    </p>
    <p class="mb-8 font-mono text-xs tracking-[0.25em] text-slate-500 uppercase">
      <DecryptText text={THEME} tag="span" duration={300} steps={8} />
    </p>

    <!-- Tagline -->
    <p class="mb-10 font-sans text-lg font-light text-slate-300 leading-relaxed">
      <DecryptText text={TAGLINE} tag="span" duration={600} steps={18} class="text-white/80" />
    </p>

    <!-- CTA Buttons -->
    <div class="flex flex-col items-center gap-4 sm:flex-row sm:justify-center">
      <!-- Primary CTA -->
      <a
        href="#register"
        on:click={scrollToRegister}
        class="
          group relative inline-block overflow-hidden rounded
          border border-[#00F0FF] px-8 py-3
          font-mono text-sm tracking-widest text-[#00F0FF] uppercase
          transition-all duration-300
          hover:bg-[#00F0FF]/10 hover:shadow-[0_0_24px_rgba(0,240,255,0.4)]
          focus:outline-none focus:ring-2 focus:ring-[#00F0FF]/50
        "
      >
        <DecryptText text="[ INITIALIZE ]" tag="span" duration={300} steps={10} />
      </a>

      <!-- Secondary CTA -->
      <a
        href="#about"
        class="
          inline-block rounded
          border border-white/10 px-8 py-3
          font-mono text-sm tracking-widest text-slate-400 uppercase
          transition-all duration-300
          hover:border-white/30 hover:text-slate-200
          focus:outline-none focus:ring-2 focus:ring-white/20
        "
      >
        <DecryptText text="LEARN MORE" tag="span" duration={300} steps={10} />
      </a>
    </div>

    <!-- Animated bottom connector -->
    <div class="mt-12 flex justify-center">
      <div class="flex flex-col items-center gap-1 opacity-40">
        <span class="h-6 w-px bg-[#00F0FF]"></span>
        <span class="h-1.5 w-1.5 rounded-full bg-[#00F0FF]"></span>
        <span class="h-4 w-px bg-[#00F0FF]"></span>
      </div>
    </div>
  </GlassCard>
</section>
