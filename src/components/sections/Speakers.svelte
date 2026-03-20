<script lang="ts">
  import GlassCard from '../ui/GlassCard.svelte';
  import DecryptText from '../ui/DecryptText.svelte';
  import CodecPortrait from '../ui/CodecPortrait.svelte';

  type Glow = 'cyan' | 'magenta' | 'none';

  interface Speaker {
    initials: string;
    colorSeed: number;
    handle: string;
    realName: string;
    role: string;
    topic: string;
    bio: string;
    glow: Glow;
    photo?: string;
    workshopTag?: string;
  }

  const SPEAKERS: Speaker[] = [
    {
      initials: 'KA',
      colorSeed: 0,
      handle: 'L1QUID_KARL',
      realName: 'Karl',
      role: 'ZK Protocol Researcher',
      topic: 'Recursive STARKs & the limits of succinctness',
      bio: 'Lead researcher at an anon ZK lab. Author of three widely-cited papers on recursive proof composition.',
      glow: 'cyan',
      photo: '/images/speakers/karl.jpeg',
      workshopTag: 'WORKSHOP A',
    },
    {
      initials: 'CA',
      colorSeed: 2,
      handle: 'C40LAN',
      realName: 'Caolan',
      role: 'Cryptographic Systems Architect',
      topic: 'Building trustless identity on-chain',
      bio: 'Former DARPA contractor turned cypherpunk. Ships ZK-identity tooling for sovereign individuals.',
      glow: 'magenta',
      photo: '/images/speakers/caolan.jpeg',
      workshopTag: 'WORKSHOP B',
    },
    {
      initials: 'QN',
      colorSeed: 5,
      handle: 'QU1NN_ZERO',
      realName: 'Quinn',
      role: 'Smart Contract Auditor',
      topic: 'Attack vectors in ZK rollup bridges',
      bio: 'Bug bounty hunter with $4M+ in critical findings. Convinced every bridge is a liability.',
      glow: 'cyan',
      photo: '/images/speakers/quinn.jpeg',
    },
    {
      initials: 'SN',
      colorSeed: 3,
      handle: 'SNAKE_NODE',
      realName: '[REDACTED]',
      role: 'P2P Network Engineer',
      topic: 'Noise Protocol & covert mesh networking',
      bio: 'Wrote the first production Noise XX implementation in Rust. Deeply suspicious of TCP.',
      glow: 'none',
    },
    {
      initials: 'EK',
      colorSeed: 4,
      handle: 'ENC_KEEPER',
      realName: '[REDACTED]',
      role: 'Applied Cryptographer',
      topic: 'MPC wallets: the promise and the pitfall',
      bio: 'PhD in lattice-based cryptography. Builds threshold signature schemes that survive quantum adversaries.',
      glow: 'magenta',
    },
    {
      initials: 'GX',
      colorSeed: 1,
      handle: 'GHOST_X',
      realName: 'G. Xuan',
      role: 'DeFi Protocol Designer',
      topic: 'Privacy-preserving AMMs with ZK proofs',
      bio: 'Designed three production AMMs. Currently obsessed with making MEV extraction cryptographically impossible.',
      glow: 'cyan',
    },
  ];

  // Workshops are speakers that have a workshopTag
  const WORKSHOPS = SPEAKERS.filter(s => s.workshopTag);
</script>

<section id="speakers" class="relative z-10 px-4 py-24">
  <div class="mx-auto max-w-5xl">

    <!-- Section header -->
    <div class="mb-14 text-center">
      <p class="mb-2 font-mono text-xs tracking-[0.4em] text-[#00F0FF]/60 uppercase">
        // SECTION 0x05
      </p>
      <h2 class="font-mono text-3xl font-bold text-white md:text-4xl">
        <DecryptText text="OPERATIVES_ON_SITE" tag="span" duration={450} steps={14} class="glow-cyan" />
      </h2>
      <p class="mt-3 font-sans text-sm text-slate-500">
        Confirmed speakers &amp; workshop leads — identities partially redacted until event day.
      </p>
      <div class="mx-auto mt-4 h-px w-24 bg-gradient-to-r from-transparent via-[#00F0FF]/50 to-transparent"></div>
    </div>

    <!-- ── SPEAKER GRID ── -->
    <div class="mb-20 grid gap-6 sm:grid-cols-2 lg:grid-cols-3">
      {#each SPEAKERS as speaker}
        <GlassCard glow={speaker.glow} padding="p-5">
          <div class="flex items-start gap-4">

            <!-- Codec portrait -->
            <div class="shrink-0">
              <CodecPortrait
                initials={speaker.initials}
                colorSeed={speaker.colorSeed}
                size={80}
                src={speaker.photo}
              />
              {#if speaker.workshopTag}
                <div class="mt-1.5 text-center">
                  <span class="font-mono text-[9px] tracking-widest text-[#FF003C]/70 uppercase">
                    {speaker.workshopTag}
                  </span>
                </div>
              {/if}
            </div>

            <!-- Info -->
            <div class="min-w-0 flex-1">
              <p class="font-mono text-[10px] tracking-widest text-slate-500 uppercase mb-0.5">
                {speaker.role}
              </p>
              <h3 class="font-mono text-base font-bold text-white leading-tight">
                <DecryptText text={speaker.handle} tag="span" duration={380} steps={12} class="glow-cyan" />
              </h3>
              <p class="mt-0.5 font-mono text-[11px] text-slate-500">// {speaker.realName}</p>
              <p class="mt-2 font-sans text-xs leading-relaxed text-slate-400">{speaker.bio}</p>
            </div>
          </div>

          <!-- Talk topic -->
          <div class="mt-4 border-t border-white/5 pt-3">
            <span class="font-mono text-[9px] tracking-widest text-[#00F0FF]/50 uppercase">
              TALK TOPIC —
            </span>
            <p class="mt-1 font-mono text-xs text-slate-300 leading-relaxed">
              "{speaker.topic}"
            </p>
          </div>
        </GlassCard>
      {/each}
    </div>

    <!-- ── WORKSHOPS DEEP-DIVE ── -->
    <div class="mb-6 flex items-center gap-4">
      <span class="font-mono text-xs tracking-widest text-[#FF003C]/70 uppercase">
        // WORKSHOPS.LIST
      </span>
      <span class="h-px flex-1 bg-white/5"></span>
    </div>

    <div class="grid gap-6 sm:grid-cols-2">
      {#each WORKSHOPS as ws, i}
        <GlassCard glow={i % 2 === 0 ? 'cyan' : 'magenta'} padding="p-6">
          <div class="mb-4 flex items-center gap-4">
            <CodecPortrait initials={ws.initials} colorSeed={ws.colorSeed} size={60} src={ws.photo} />
            <div>
              <span class="font-mono text-[10px] tracking-widest text-[#FF003C]/70 uppercase">
                {ws.workshopTag}
              </span>
              <p class="font-mono text-sm font-bold text-white leading-tight mt-0.5">
                <DecryptText text={ws.handle} tag="span" duration={320} steps={10} />
              </p>
              <p class="font-mono text-[10px] text-slate-500">// {ws.realName}</p>
            </div>
          </div>

          <h4 class="mb-2 font-mono text-sm font-bold text-[#00F0FF]">
            {ws.topic}
          </h4>
          <p class="font-sans text-xs leading-relaxed text-slate-400">{ws.bio}</p>

          <div class="mt-4 flex items-center gap-2">
            <span class="h-1.5 w-1.5 rounded-full bg-[#00F0FF] shadow-[0_0_6px_#00F0FF]"></span>
            <span class="font-mono text-[10px] text-slate-500">
              {i === 0 ? 'SAT 18 APR · 09:00 UTC' : 'SAT 18 APR · 14:00 UTC'}
            </span>
          </div>
        </GlassCard>
      {/each}
    </div>

  </div>
</section>
