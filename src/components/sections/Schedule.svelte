<script lang="ts">
  import GlassCard from '../ui/GlassCard.svelte';
  import DecryptText from '../ui/DecryptText.svelte';

  interface ScheduleEntry {
    time: string;
    day: string;
    event: string;
    detail: string;
    accent: 'cyan' | 'magenta' | 'neutral';
  }

  const ENTRIES: ScheduleEntry[] = [
    {
      time: '18:00',
      day: 'FRI 17 APR',
      event: 'NODE GENESIS',
      detail: 'Registration opens. Participant onboarding, wallet verification, and team formation.',
      accent: 'cyan',
    },
    {
      time: '20:00',
      day: 'FRI 17 APR',
      event: 'CEREMONY INIT',
      detail: 'Opening keynote: "The Cryptographic Social Contract". Hack begins at 21:00.',
      accent: 'magenta',
    },
    {
      time: '09:00',
      day: 'SAT 18 APR',
      event: 'WORKSHOP A',
      detail: 'ZK-SNARKs 101 with Circom + SnarkJS. Hands-on circuit construction.',
      accent: 'cyan',
    },
    {
      time: '14:00',
      day: 'SAT 18 APR',
      event: 'WORKSHOP B',
      detail: 'Privacy-preserving identity systems. DIDs, Verifiable Credentials, and ZK rollups.',
      accent: 'neutral',
    },
    {
      time: '00:00',
      day: 'SUN 19 APR',
      event: 'MIDNIGHT SIGNAL',
      detail: 'Mid-point check-in. Optional mentor sessions. The darkest hour before the proof.',
      accent: 'magenta',
    },
    {
      time: '12:00',
      day: 'SUN 19 APR',
      event: 'SUBMISSION LOCK',
      detail: 'Final commit deadline. All repos tagged. No force-pushes after this block.',
      accent: 'cyan',
    },
    {
      time: '15:00',
      day: 'SUN 19 APR',
      event: 'CIRCUIT DEMOS',
      detail: 'Live 5-minute demos followed by 2-minute Q&A. Judging begins immediately.',
      accent: 'neutral',
    },
    {
      time: '18:00',
      day: 'SUN 19 APR',
      event: 'PROOF VERIFIED',
      detail: 'Awards ceremony. Prizes distributed on-chain. The protocol continues.',
      accent: 'magenta',
    },
  ];

  type AccentType = 'cyan' | 'magenta' | 'neutral';

  const ACCENT_COLORS: Record<AccentType, string> = {
    cyan:    '#00F0FF',
    magenta: '#FF003C',
    neutral: 'rgba(255,255,255,0.2)',
  };
</script>

<section id="schedule" class="relative z-10 px-4 py-24">
  <div class="mx-auto max-w-4xl">
    <!-- Section header -->
    <div class="mb-14 text-center">
      <p class="mb-2 font-mono text-xs tracking-[0.4em] text-[#00F0FF]/60 uppercase">
        // SECTION 0x03
      </p>
      <h2 class="font-mono text-3xl font-bold text-white md:text-4xl">
        <DecryptText text="EXECUTION_SCHEDULE" tag="span" duration={450} steps={14} class="glow-cyan" />
      </h2>
      <div class="mx-auto mt-4 h-px w-24 bg-gradient-to-r from-transparent via-[#00F0FF]/50 to-transparent"></div>
    </div>

    <!-- Timeline -->
    <div class="relative">
      <!-- Vertical spine -->
      <div
        class="absolute left-[72px] top-0 h-full w-px hidden sm:block"
        style="background: linear-gradient(to bottom, transparent, rgba(0,240,255,0.3) 10%, rgba(0,240,255,0.3) 90%, transparent);"
      ></div>

      <div class="space-y-4">
        {#each ENTRIES as entry, i}
          <div class="flex gap-4 sm:gap-6">
            <!-- Time stamp column -->
            <div class="hidden w-[68px] shrink-0 flex-col items-end justify-start pt-4 sm:flex">
              <span class="font-mono text-xs text-slate-500">{entry.day.split(' ')[0]}</span>
              <span
                class="font-mono text-lg font-bold leading-none"
                style="color: {ACCENT_COLORS[entry.accent]};"
              >{entry.time}</span>
            </div>

            <!-- Connector dot (desktop only) -->
            <div class="relative hidden w-0 items-start justify-center pt-5 sm:flex">
              <div
                class="absolute h-2.5 w-2.5 -translate-x-1/2 rounded-full ring-2 ring-[#0B0E14]"
                style="background: {ACCENT_COLORS[entry.accent]}; box-shadow: 0 0 8px {ACCENT_COLORS[entry.accent]}80;"
              ></div>
            </div>

            <!-- Card -->
            <div class="min-w-0 flex-1">
              <GlassCard glow={entry.accent === 'neutral' ? 'none' : entry.accent} padding="p-4">
                <div class="mb-1 flex flex-wrap items-baseline gap-3">
                  <!-- Mobile: show time inline -->
                  <span class="font-mono text-sm font-bold sm:hidden" style="color: {ACCENT_COLORS[entry.accent]};">
                    {entry.time}
                  </span>
                  <span class="font-mono text-[10px] tracking-widest text-slate-500 uppercase sm:hidden">
                    {entry.day}
                  </span>
                  <span class="font-mono text-[10px] tracking-widest text-slate-500 uppercase hidden sm:inline">
                    {entry.day}
                  </span>
                </div>
                <h3 class="mb-1.5 font-mono text-base font-bold text-white">
                  <DecryptText text={entry.event} tag="span" duration={320} steps={10} />
                </h3>
                <p class="font-sans text-sm leading-relaxed text-slate-400">{entry.detail}</p>
              </GlassCard>
            </div>
          </div>
        {/each}
      </div>
    </div>
  </div>
</section>
