<script lang="ts">
  /**
   * CodecPortrait — renders a speaker avatar as a Metal Gear Solid–style
   * codec transmission portrait: scanlines, CRT noise, interlace flicker,
   * and a green phosphor colour-shift — all in pure CSS + SVG.
   *
   * Props:
   *   initials  - 1-2 character fallback rendered as the "face" glyph
   *   colorSeed - number 0-5 to pick a phosphor palette variant
   *   size      - diameter in px (default 96)
   *   src       - optional real photo path; applies full MGS codec filter treatment
   */
  export let initials: string = '??';
  export let colorSeed: number = 0;
  export let size: number = 96;
  export let src: string | undefined = undefined;

  // Phosphor palette variants inspired by MGS codec screens
  const PALETTES = [
    { bg: '#0a1a0a', mid: '#1a4a1a', hi: '#39ff14', rim: '#00ff41' }, // P1 green
    { bg: '#0a0f1a', mid: '#1a2a4a', hi: '#00f0ff', rim: '#00c8ff' }, // P2 cyan
    { bg: '#1a0a0f', mid: '#3a1020', hi: '#ff003c', rim: '#ff2255' }, // P3 red
    { bg: '#0f0a1a', mid: '#2a1a3a', hi: '#b060ff', rim: '#9040ee' }, // P4 violet
    { bg: '#1a100a', mid: '#3a2010', hi: '#ff8c00', rim: '#ffaa00' }, // P5 amber
    { bg: '#0a1a17', mid: '#0d3a30', hi: '#00ffcc', rim: '#00ddaa' }, // P6 teal
  ];

  const p = PALETTES[colorSeed % PALETTES.length];

  // Deterministic "face" geometry from initials charCodes
  function seed(s: string): number {
    return s.split('').reduce((acc, c) => acc + c.charCodeAt(0), 0);
  }
  const n = seed(initials);

  // hair stripe y positions (3 stripes)
  const hair = [
    8  + (n % 5),
    13 + (n % 3),
    18 + (n % 7),
  ];

  // unique id so multiple instances don't clash
  const uid = `cp-${initials.replace(/[^a-z0-9]/gi, '')}-${colorSeed}`;

  // Phosphor tint matrices per palette — maps image luminance to the palette's hi colour
  // Format: feColorMatrix type="matrix" R-row G-row B-row A-row (space-separated)
  const PHOSPHOR_MATRICES: Record<number, string> = {
    0: '0.1 0.2 0.05 0 0   0.3 0.7 0.15 0 0.04   0.02 0.08 0.02 0 0   0 0 0 1 0',  // green
    1: '0.02 0.1 0.15 0 0   0.1 0.3 0.5 0 0.02   0.15 0.4 0.6 0 0.04  0 0 0 1 0',  // cyan
    2: '0.5 0.25 0.1 0 0.04 0.02 0.08 0.04 0 0   0.02 0.05 0.02 0 0   0 0 0 1 0',  // red
    3: '0.15 0.1 0.3 0 0    0.05 0.05 0.2 0 0    0.4 0.3 0.7 0 0.02   0 0 0 1 0',  // violet
    4: '0.5 0.3 0.1 0 0.04  0.25 0.2 0.05 0 0.01 0.02 0.02 0.01 0 0   0 0 0 1 0',  // amber
    5: '0.05 0.1 0.15 0 0   0.2 0.5 0.4 0 0.04   0.1 0.3 0.35 0 0.02  0 0 0 1 0',  // teal
  };
  const phosphorMatrix = PHOSPHOR_MATRICES[colorSeed % 6];
</script>

<div
  class="codec-portrait relative select-none overflow-hidden rounded"
  style="
    width: {size}px;
    height: {size}px;
    background: {p.bg};
    box-shadow: 0 0 0 1px {p.rim}55, 0 0 14px {p.rim}44;
    image-rendering: pixelated;
  "
  aria-hidden="true"
>
  <svg
    width={size}
    height={size}
    viewBox="0 0 64 64"
    xmlns="http://www.w3.org/2000/svg"
  >
    <defs>
      <!-- Scanline pattern -->
      <pattern id="{uid}-scan" x="0" y="0" width="1" height="2" patternUnits="userSpaceOnUse">
        <rect x="0" y="0" width="64" height="1" fill="rgba(0,0,0,0.35)" />
      </pattern>

      <!-- Noise / grain filter -->
      <filter id="{uid}-noise">
        <feTurbulence type="fractalNoise" baseFrequency="0.85" numOctaves="4" seed={n % 99} result="noise" />
        <feColorMatrix in="noise" type="saturate" values="0" result="grey" />
        <feBlend in="SourceGraphic" in2="grey" mode="overlay" result="blend" />
        <feComponentTransfer in="blend">
          <feFuncA type="linear" slope="0.12" />
        </feComponentTransfer>
        <feComposite in="SourceGraphic" operator="over" />
      </filter>

      <!-- CRT phosphor glow -->
      <filter id="{uid}-glow">
        <feGaussianBlur stdDeviation="1.2" result="blur" />
        <feMerge>
          <feMergeNode in="blur" />
          <feMergeNode in="SourceGraphic" />
        </feMerge>
      </filter>

      {#if src}
        <!--
          Photo codec filter pipeline:
          1. Desaturate to near-monochrome
          2. Boost contrast via component transfer (harsh codec look)
          3. Apply phosphor tint matrix
          4. Slight phosphor glow blur
        -->
        <filter id="{uid}-codec" color-interpolation-filters="sRGB" x="0" y="0" width="100%" height="100%">
          <!-- Step 1: desaturate -->
          <feColorMatrix type="saturate" values="0" result="grey" />
          <!-- Step 2: harsh contrast curve (codec grit) -->
          <feComponentTransfer in="grey" result="contrast">
            <feFuncR type="gamma" amplitude="1" exponent="0.75" offset="-0.05" />
            <feFuncG type="gamma" amplitude="1" exponent="0.75" offset="-0.05" />
            <feFuncB type="gamma" amplitude="1" exponent="0.75" offset="-0.05" />
          </feComponentTransfer>
          <!-- Step 3: phosphor tint -->
          <feColorMatrix in="contrast" type="matrix" values={phosphorMatrix} result="tinted" />
          <!-- Step 4: subtle glow -->
          <feGaussianBlur in="tinted" stdDeviation="0.4" result="glow" />
          <feMerge>
            <feMergeNode in="glow" />
            <feMergeNode in="tinted" />
          </feMerge>
        </filter>

        <!-- Clip path to match portrait bounds -->
        <clipPath id="{uid}-clip">
          <rect width="64" height="64" />
        </clipPath>
      {/if}
    </defs>

    <!-- Background fill -->
    <rect width="64" height="64" fill={p.bg} />

    <!-- Vignette gradient def -->
    <radialGradient id="{uid}-vig" cx="50%" cy="50%" r="70%">
      <stop offset="40%" stop-color="transparent" />
      <stop offset="100%" stop-color="rgba(0,0,0,0.75)" />
    </radialGradient>

    {#if src}
      <!-- ── PHOTO MODE: real image with full codec SVG filter treatment ── -->

      <!-- Photo with phosphor codec filter applied -->
      <image
        href={src}
        x="0" y="0"
        width="64" height="64"
        preserveAspectRatio="xMidYMid slice"
        clip-path="url(#{uid}-clip)"
        filter="url(#{uid}-codec)"
      />

      <!-- Shoulder / collar dark overlay to frame the face like a codec portrait -->
      <path d="M0 64 L0 52 Q10 46 20 48 L44 48 Q54 46 64 52 L64 64 Z" fill={p.bg} opacity="0.85" />
      <path d="M20 48 L32 54 L44 48" fill="none" stroke={p.hi} stroke-width="0.8" opacity="0.6" />

      <!-- Top dark bar (header area) -->
      <rect x="0" y="0" width="64" height="6" fill={p.bg} opacity="0.6" />

    {:else}
      <!-- ── GEOMETRY MODE: abstract face (no photo) ── -->

      <!-- Neck -->
      <rect x="26" y="46" width="12" height="12" rx="1" fill={p.mid} />

      <!-- Head shape -->
      <rect x="18" y="20" width="28" height="28" rx="4" fill={p.mid} filter="url(#{uid}-glow)" />

      <!-- Highlight on face -->
      <rect x="20" y="22" width="12" height="8" rx="2" fill={p.hi} opacity="0.18" />

      <!-- Eyes -->
      <rect x="22" y="31" width="6" height="4" rx="1" fill={p.hi} opacity="0.85" filter="url(#{uid}-glow)" />
      <rect x="36" y="31" width="6" height="4" rx="1" fill={p.hi} opacity="0.85" filter="url(#{uid}-glow)" />

      <!-- Eye shine dots -->
      <rect x="24" y="32" width="2" height="2" fill="white" opacity="0.5" />
      <rect x="38" y="32" width="2" height="2" fill="white" opacity="0.5" />

      <!-- Mouth -->
      <rect x="27" y="40" width="10" height="2" rx="1" fill={p.hi} opacity="0.55" />

      <!-- Hair stripes (deterministic from initials) -->
      {#each hair as hy, i}
        <rect x="18" y={hy} width={28 - i * 4} height="2" rx="0.5" fill={p.hi} opacity={0.5 - i * 0.1} />
      {/each}

      <!-- Shoulder silhouette -->
      <path d="M8 64 L8 58 Q16 52 24 52 L40 52 Q48 52 56 58 L56 64 Z" fill={p.mid} />
      <!-- Collar accent -->
      <path d="M24 52 L32 58 L40 52" fill="none" stroke={p.hi} stroke-width="1" opacity="0.5" />
    {/if}

    <!-- shared overlays: scanlines, noise, vignette, flicker — always present -->

    <!-- Scanline overlay -->
    <rect width="64" height="64" fill="url(#{uid}-scan)" />

    <!-- Noise overlay -->
    <rect width="64" height="64" fill="transparent" filter="url(#{uid}-noise)" opacity="0.4" />

    <!-- Vignette overlay -->
    <rect width="64" height="64" fill="url(#{uid}-vig)" />

    <!-- Initials tag in bottom-left corner (photo mode only, more visible) -->
    {#if src}
      <text x="3" y="62" font-family="monospace" font-size="5" fill={p.hi} opacity="0.5" letter-spacing="1">{initials}</text>
    {/if}

    <!-- Interlace flicker line (animated) -->
    <rect x="0" y="0" width="64" height="1" fill={p.hi} opacity="0.08">
      <animateTransform
        attributeName="transform"
        type="translate"
        values="0 0; 0 64; 0 0"
        dur="3s"
        begin="0s"
        repeatCount="indefinite"
        calcMode="linear"
      />
      <animate attributeName="opacity" values="0.08;0.15;0;0.08" dur="3s" repeatCount="indefinite" />
    </rect>
  </svg>

  <!-- CSS scanline shimmer (second pass, pure CSS) -->
  <div
    class="pointer-events-none absolute inset-0"
    style="
      background: repeating-linear-gradient(
        0deg,
        transparent,
        transparent 1px,
        rgba(0,0,0,0.18) 1px,
        rgba(0,0,0,0.18) 2px
      );
    "
  ></div>

  <!-- Transmission flicker animation -->
  <div class="codec-flicker pointer-events-none absolute inset-0 rounded" style="border: 1px solid {p.rim}66;"></div>
</div>

<style>
  .codec-flicker {
    animation: codec-flicker 4s steps(1) infinite;
  }

  @keyframes codec-flicker {
    0%   { opacity: 1; }
    92%  { opacity: 1; }
    93%  { opacity: 0.5; }
    94%  { opacity: 1; }
    96%  { opacity: 0.7; }
    97%  { opacity: 1; }
    100% { opacity: 1; }
  }
</style>
