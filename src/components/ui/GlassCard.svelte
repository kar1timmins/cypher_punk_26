<script lang="ts">
  /**
   * GlassCard — reusable frosted-glass container.
   *
   * Props:
   *   class   - extra Tailwind classes to pass to the outer element
   *   padding - inner padding utility class (default "p-6")
   *   glow    - "cyan" | "magenta" | "none" — controls the border/shadow accent colour
   */
  export let padding: string = 'p-6';
  export let glow: 'cyan' | 'magenta' | 'none' = 'cyan';
  let extra = '';
  export { extra as class };
</script>

<div
  class="
    relative rounded-lg overflow-hidden
    bg-black/20 backdrop-blur-md
    border border-transparent
    {glow === 'cyan'    ? 'shadow-[0_0_18px_rgba(0,240,255,0.12)]' : ''}
    {glow === 'magenta' ? 'shadow-[0_0_18px_rgba(255,0,60,0.12)]'  : ''}
    {padding}
    {extra}
  "
  style="
    border-image: none;
    border-top-color:  {glow === 'cyan' ? 'rgba(0,240,255,0.35)' : glow === 'magenta' ? 'rgba(255,0,60,0.35)'  : 'rgba(255,255,255,0.06)'};
    border-left-color: {glow === 'cyan' ? 'rgba(0,240,255,0.2)'  : glow === 'magenta' ? 'rgba(255,0,60,0.2)'   : 'rgba(255,255,255,0.04)'};
    border-right-color:  rgba(255,255,255,0.04);
    border-bottom-color: rgba(255,255,255,0.04);
  "
>
  <!-- inner highlight strip -->
  <div
    class="pointer-events-none absolute inset-0 rounded-lg"
    style="
      background: linear-gradient(
        135deg,
        {glow === 'cyan'    ? 'rgba(0,240,255,0.06)' :
         glow === 'magenta' ? 'rgba(255,0,60,0.06)'  : 'rgba(255,255,255,0.03)'} 0%,
        transparent 60%
      );
    "
  ></div>

  <slot />
</div>
