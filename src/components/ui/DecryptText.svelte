<script lang="ts">
  /**
   * DecryptText — hover-triggered hex-scramble reveal mechanic.
   *
   * Props:
   *   text      - the final string to reveal
   *   tag       - HTML element to render as (default "span")
   *   duration  - total scramble duration in ms (default 420)
   *   steps     - how many scramble frames (default 14)
   *   class     - extra Tailwind classes
   */
  export let text: string;
  export let tag: 'span' | 'h1' | 'h2' | 'h3' | 'p' | 'div' = 'span';
  export let duration: number = 420;
  export let steps: number = 14;
  let extra = '';
  export { extra as class };

  const HEX_CHARS = '0123456789ABCDEF';
  const SYMBOLS   = '!@#$%^&*<>/\\|{}[]~`';
  const POOL      = HEX_CHARS + SYMBOLS;

  let displayed: string = text;
  let isDecrypting = false;
  let timerId: ReturnType<typeof setInterval> | null = null;

  function randomChar(): string {
    return POOL[Math.floor(Math.random() * POOL.length)];
  }

  /**
   * For each character position in the target string, decide whether it has
   * been "resolved" yet based on the current frame index.  Characters resolve
   * from left-to-right so the effect feels directional.
   */
  function buildFrame(frame: number): string {
    const total = text.length;
    // fraction of chars that are already locked in this frame
    const locked = Math.round((frame / steps) * total);
    return text
      .split('')
      .map((ch, i) => {
        if (ch === ' ') return ' ';
        if (i < locked) return ch;
        return randomChar();
      })
      .join('');
  }

  function startDecrypt() {
    if (isDecrypting) return;
    isDecrypting = true;
    let frame = 0;
    const interval = duration / steps;

    timerId = setInterval(() => {
      frame++;
      if (frame >= steps) {
        displayed = text;
        isDecrypting = false;
        if (timerId !== null) clearInterval(timerId);
        timerId = null;
      } else {
        displayed = buildFrame(frame);
      }
    }, interval);
  }

  function resetDecrypt() {
    // Let the animation finish naturally; don't interrupt.
  }
</script>

<!-- svelte-ignore a11y-no-static-element-interactions -->
<svelte:element
  this={tag}
  class="font-mono tracking-wider cursor-default select-none {extra}"
  on:mouseenter={startDecrypt}
  on:mouseleave={resetDecrypt}
>{displayed}</svelte:element>
