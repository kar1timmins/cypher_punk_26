<script lang="ts">
  import GlassCard from '../ui/GlassCard.svelte';
  import DecryptText from '../ui/DecryptText.svelte';

  // Form state
  let handle   = '';
  let email    = '';
  let track    = '';
  let teamSize = '1';
  let wallet   = '';
  let agreed   = false;

  type FormStatus = 'idle' | 'validating' | 'success' | 'error';
  let status: FormStatus = 'idle';
  let statusMsg = '';

  const TRACKS = [
    { value: '',           label: '>> SELECT TRACK <<' },
    { value: 'zk-identity', label: 'ZK Identity & Credentials' },
    { value: 'zk-rollup',   label: 'ZK Rollup / L2 Scaling'   },
    { value: 'zk-voting',   label: 'Private Voting Systems'    },
    { value: 'zk-defi',     label: 'Privacy-Preserving DeFi'  },
    { value: 'zk-open',     label: 'Open Category'             },
  ];

  // Strict wallet address validation (ETH-style 0x + 40 hex chars)
  function isValidWalletAddress(addr: string): boolean {
    return /^0x[0-9a-fA-F]{40}$/.test(addr.trim());
  }

  // Basic email validation
  function isValidEmail(addr: string): boolean {
    return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(addr.trim());
  }

  function handleSubmit(e: SubmitEvent) {
    e.preventDefault();
    status = 'validating';
    statusMsg = '';

    if (!handle.trim()) {
      status = 'error';
      statusMsg = 'ERR: handle cannot be null.';
      return;
    }
    if (!isValidEmail(email)) {
      status = 'error';
      statusMsg = 'ERR: invalid email format.';
      return;
    }
    if (!track) {
      status = 'error';
      statusMsg = 'ERR: track selection required.';
      return;
    }
    if (wallet && !isValidWalletAddress(wallet)) {
      status = 'error';
      statusMsg = 'ERR: wallet address format invalid (expected 0x…40 hex chars).';
      return;
    }
    if (!agreed) {
      status = 'error';
      statusMsg = 'ERR: you must accept the protocol terms.';
      return;
    }

    // Simulate async submission (replace with real endpoint)
    setTimeout(() => {
      status = 'success';
      statusMsg = 'OK: registration packet transmitted. Stand by for confirmation.';
    }, 800);
  }
</script>

<section id="register" class="relative z-10 px-4 py-24 pb-32">
  <div class="mx-auto max-w-2xl">
    <!-- Section header -->
    <div class="mb-14 text-center">
      <p class="mb-2 font-mono text-xs tracking-[0.4em] text-[#00F0FF]/60 uppercase">
        // SECTION 0x04
      </p>
      <h2 class="font-mono text-3xl font-bold text-white md:text-4xl">
        <DecryptText text="REGISTER.SH" tag="span" duration={450} steps={14} class="glow-cyan" />
      </h2>
      <div class="mx-auto mt-4 h-px w-24 bg-gradient-to-r from-transparent via-[#00F0FF]/50 to-transparent"></div>
    </div>

    <GlassCard glow="cyan" padding="p-8">
      <!-- Terminal header bar -->
      <div class="mb-6 flex items-center gap-2 border-b border-white/5 pb-4">
        <span class="h-2.5 w-2.5 rounded-full bg-[#FF003C]/70"></span>
        <span class="h-2.5 w-2.5 rounded-full bg-yellow-400/50"></span>
        <span class="h-2.5 w-2.5 rounded-full bg-[#00F0FF]/50"></span>
        <span class="ml-3 font-mono text-xs text-slate-500">register@cypher-punk-26 ~ $</span>
      </div>

      {#if status === 'success'}
        <!-- Success state -->
        <div class="py-8 text-center">
          <p class="mb-3 font-mono text-2xl text-[#00F0FF] glow-cyan">[ OK ]</p>
          <p class="font-mono text-sm text-slate-300">{statusMsg}</p>
          <p class="mt-4 font-mono text-xs text-slate-500">
            // You will receive encrypted confirmation within 24h.
          </p>
        </div>
      {:else}
        <form on:submit={handleSubmit} novalidate class="space-y-5">

          <!-- Handle -->
          <div class="space-y-1">
            <label for="reg-handle" class="block font-mono text-xs tracking-widest text-slate-500 uppercase">
              $ handle —
            </label>
            <input
              id="reg-handle"
              type="text"
              bind:value={handle}
              maxlength="32"
              placeholder="anon_operator_42"
              autocomplete="username"
              class="
                w-full rounded border border-white/10 bg-black/30
                px-4 py-2.5 font-mono text-sm text-[#00F0FF]
                placeholder-slate-600 outline-none
                transition-colors duration-200
                focus:border-[#00F0FF]/50 focus:ring-1 focus:ring-[#00F0FF]/30
              "
            />
          </div>

          <!-- Email -->
          <div class="space-y-1">
            <label for="reg-email" class="block font-mono text-xs tracking-widest text-slate-500 uppercase">
              $ email —
            </label>
            <input
              id="reg-email"
              type="email"
              bind:value={email}
              placeholder="operator@proton.me"
              autocomplete="email"
              class="
                w-full rounded border border-white/10 bg-black/30
                px-4 py-2.5 font-mono text-sm text-[#00F0FF]
                placeholder-slate-600 outline-none
                transition-colors duration-200
                focus:border-[#00F0FF]/50 focus:ring-1 focus:ring-[#00F0FF]/30
              "
            />
          </div>

          <!-- Track + Team size (two-col) -->
          <div class="grid gap-4 sm:grid-cols-2">
            <div class="space-y-1">
              <label for="reg-track" class="block font-mono text-xs tracking-widest text-slate-500 uppercase">
                $ track —
              </label>
              <select
                id="reg-track"
                bind:value={track}
                class="
                  w-full rounded border border-white/10 bg-[#0B0E14]
                  px-4 py-2.5 font-mono text-sm text-[#00F0FF]
                  outline-none appearance-none
                  transition-colors duration-200
                  focus:border-[#00F0FF]/50 focus:ring-1 focus:ring-[#00F0FF]/30
                "
              >
                {#each TRACKS as t}
                  <option value={t.value}>{t.label}</option>
                {/each}
              </select>
            </div>

            <div class="space-y-1">
              <label for="reg-team" class="block font-mono text-xs tracking-widest text-slate-500 uppercase">
                $ team_size —
              </label>
              <select
                id="reg-team"
                bind:value={teamSize}
                class="
                  w-full rounded border border-white/10 bg-[#0B0E14]
                  px-4 py-2.5 font-mono text-sm text-[#00F0FF]
                  outline-none appearance-none
                  transition-colors duration-200
                  focus:border-[#00F0FF]/50 focus:ring-1 focus:ring-[#00F0FF]/30
                "
              >
                <option value="1">1 — Solo operator</option>
                <option value="2">2 — Duo</option>
                <option value="3">3 — Triad</option>
                <option value="4">4 — Squad (max)</option>
              </select>
            </div>
          </div>

          <!-- Wallet (optional) -->
          <div class="space-y-1">
            <label for="reg-wallet" class="block font-mono text-xs tracking-widest text-slate-500 uppercase">
              $ wallet_address <span class="text-slate-600">(optional)</span> —
            </label>
            <input
              id="reg-wallet"
              type="text"
              bind:value={wallet}
              placeholder="0x..."
              maxlength="42"
              autocomplete="off"
              spellcheck="false"
              class="
                w-full rounded border border-white/10 bg-black/30
                px-4 py-2.5 font-mono text-sm text-[#00F0FF]
                placeholder-slate-600 outline-none
                transition-colors duration-200
                focus:border-[#00F0FF]/50 focus:ring-1 focus:ring-[#00F0FF]/30
              "
            />
            <p class="font-mono text-[10px] text-slate-600">
              // for on-chain prize distribution
            </p>
          </div>

          <!-- Terms agreement -->
          <label class="flex cursor-pointer items-start gap-3">
            <input
              type="checkbox"
              bind:checked={agreed}
              class="mt-0.5 h-4 w-4 shrink-0 accent-[#00F0FF]"
            />
            <span class="font-mono text-xs leading-relaxed text-slate-400">
              I accept the hackathon protocol terms, the open-source requirement, and acknowledge
              that submitted code will be publicly auditable.
            </span>
          </label>

          <!-- Status / error message -->
          {#if status === 'error'}
            <p class="font-mono text-xs text-[#FF003C]">{statusMsg}</p>
          {/if}

          <!-- Submit -->
          <button
            type="submit"
            disabled={status === 'validating'}
            class="
              w-full rounded border border-[#00F0FF]/60 py-3
              font-mono text-sm tracking-widest text-[#00F0FF] uppercase
              transition-all duration-300
              hover:bg-[#00F0FF]/10 hover:shadow-[0_0_20px_rgba(0,240,255,0.3)]
              disabled:cursor-not-allowed disabled:opacity-40
              focus:outline-none focus:ring-2 focus:ring-[#00F0FF]/40
            "
          >
            {status === 'validating' ? '[ VERIFYING... ]' : '[ TRANSMIT REGISTRATION ]'}
          </button>
        </form>
      {/if}
    </GlassCard>
  </div>
</section>
