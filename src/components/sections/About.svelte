<script lang="ts">
  import GlassCard from '../ui/GlassCard.svelte';
  import DecryptText from '../ui/DecryptText.svelte';

  interface Pillar {
    id: string;
    label: string;
    title: string;
    body: string;
    glow: 'cyan' | 'magenta' | 'none';
  }

  const PILLARS: Pillar[] = [
    {
      id: '0x01',
      label: 'PROTOCOL',
      title: 'Zero Knowledge',
      body: 'Prove anything without revealing everything. ZK-SNARKs and STARKs are the primitive. Your imagination is the constraint.',
      glow: 'cyan',
    },
    {
      id: '0x02',
      label: 'ETHOS',
      title: 'Radical Privacy',
      body: 'Surveillance kills freedom. We build systems where the default is cryptographic sovereignty — for every human on the network.',
      glow: 'magenta',
    },
    {
      id: '0x03',
      label: 'STACK',
      title: 'Permissionless Stack',
      body: 'Open-source tooling only. No corporate SDKs. No data brokers. Ship code that runs without asking for permission.',
      glow: 'cyan',
    },
    {
      id: '0x04',
      label: 'OUTPUT',
      title: 'Deployable Protocol',
      body: 'Presentations are theatre. We ship. Every submission must be a runnable artifact — on-chain, off-chain, or hybrid.',
      glow: 'none',
    },
  ];

  const RULES = [
    'Teams: 1–4 participants.',
    'All code must be open-source under MIT or Apache-2.0.',
    'No proprietary APIs that introduce centralized trust assumptions.',
    'ZK component is mandatory — not optional.',
    'Submission: tagged git repo + 90-second demo video.',
    'Judging: Cryptographic correctness, UX, and societal impact.',
  ];
</script>

<section id="about" class="relative z-10 px-4 py-24">
  <div class="mx-auto max-w-5xl">
    <!-- Section header -->
    <div class="mb-14 text-center">
      <p class="mb-2 font-mono text-xs tracking-[0.4em] text-[#00F0FF]/60 uppercase">
        // SECTION 0x02
      </p>
      <h2 class="font-mono text-3xl font-bold text-white md:text-4xl">
        <DecryptText text="ABOUT_THE_PROTOCOL" tag="span" duration={450} steps={14} class="glow-cyan" />
      </h2>
      <div class="mx-auto mt-4 h-px w-24 bg-gradient-to-r from-transparent via-[#00F0FF]/50 to-transparent"></div>
    </div>

    <!-- Pillar grid -->
    <div class="mb-16 grid gap-6 sm:grid-cols-2">
      {#each PILLARS as pillar}
        <GlassCard glow={pillar.glow} padding="p-6">
          <div class="mb-3 flex items-center gap-3">
            <span class="font-mono text-xs text-[#00F0FF]/50">{pillar.id}</span>
            <span class="h-px flex-1 bg-white/5"></span>
            <span class="font-mono text-[10px] tracking-widest text-slate-500 uppercase">
              {pillar.label}
            </span>
          </div>
          <h3 class="mb-3 font-mono text-lg font-bold text-white">
            <DecryptText text={pillar.title} tag="span" duration={380} steps={12} />
          </h3>
          <p class="font-sans text-sm leading-relaxed text-slate-400">{pillar.body}</p>
        </GlassCard>
      {/each}
    </div>

    <!-- Rules panel -->
    <GlassCard glow="none" padding="p-8">
      <div class="mb-6 flex items-center gap-4">
        <span class="font-mono text-xs tracking-widest text-[#FF003C]/70 uppercase">
          // RULES.md
        </span>
        <span class="h-px flex-1 bg-white/5"></span>
      </div>
      <ul class="space-y-3">
        {#each RULES as rule, i}
          <li class="flex items-start gap-3">
            <span class="mt-px font-mono text-xs text-[#FF003C]/60 shrink-0">
              {String(i + 1).padStart(2, '0')}.
            </span>
            <span class="font-sans text-sm text-slate-300">{rule}</span>
          </li>
        {/each}
      </ul>
    </GlassCard>
  </div>
</section>
