<script lang="ts">
  /**
   * CircuitMesh — fixed full-screen background layer.
   *
   * Renders:
   *   - 1px CSS mesh grid (via app.css `.mesh-grid`)
   *   - SVG circuit paths with animated glowing packets representing
   *     ZK-proof circuit wires.
   */

  // Node positions as percentages of viewport
  const NODES: Array<{ cx: number; cy: number }> = [
    { cx: 8,  cy: 12 }, { cx: 22, cy: 5  }, { cx: 40, cy: 18 },
    { cx: 58, cy: 8  }, { cx: 75, cy: 22 }, { cx: 92, cy: 10 },
    { cx: 15, cy: 38 }, { cx: 31, cy: 45 }, { cx: 49, cy: 35 },
    { cx: 66, cy: 48 }, { cx: 83, cy: 38 }, { cx: 5,  cy: 62 },
    { cx: 24, cy: 70 }, { cx: 42, cy: 58 }, { cx: 60, cy: 72 },
    { cx: 78, cy: 62 }, { cx: 95, cy: 78 }, { cx: 12, cy: 85 },
    { cx: 35, cy: 92 }, { cx: 55, cy: 82 }, { cx: 70, cy: 90 },
  ];

  // Edges connect pairs of node indices
  const EDGES: Array<[number, number]> = [
    [0,1],[1,2],[2,3],[3,4],[4,5],
    [0,6],[1,7],[2,8],[3,9],[4,10],
    [6,7],[7,8],[8,9],[9,10],
    [6,11],[7,12],[8,13],[9,14],[10,15],
    [11,12],[12,13],[13,14],[14,15],[15,16],
    [11,17],[12,18],[13,19],[14,20],[15,20],
    [17,18],[18,19],[19,20],
  ];

  // Animated pulse packets: each travels along one edge
  const PACKETS = EDGES.filter((_, i) => i % 3 === 0).map((edge, i) => ({
    edge,
    delay: i * 0.4,
    dur: 2.5 + (i % 5) * 0.5,
    color: i % 3 === 0 ? '#00F0FF' : '#FF003C',
  }));

  function pct(v: number, axis: 'x' | 'y'): string {
    return axis === 'x' ? `${v}%` : `${v}%`;
  }
</script>

<div
  aria-hidden="true"
  class="mesh-grid pointer-events-none fixed inset-0 z-0 overflow-hidden"
>
  <svg
    class="absolute inset-0 w-full h-full opacity-40"
    xmlns="http://www.w3.org/2000/svg"
    preserveAspectRatio="xMidYMid slice"
    viewBox="0 0 100 100"
  >
    <defs>
      <!-- Cyan glow filter -->
      <filter id="glow-cyan" x="-50%" y="-50%" width="200%" height="200%">
        <feGaussianBlur stdDeviation="0.4" result="blur" />
        <feMerge>
          <feMergeNode in="blur" />
          <feMergeNode in="SourceGraphic" />
        </feMerge>
      </filter>
      <!-- Magenta glow filter -->
      <filter id="glow-mag" x="-50%" y="-50%" width="200%" height="200%">
        <feGaussianBlur stdDeviation="0.4" result="blur" />
        <feMerge>
          <feMergeNode in="blur" />
          <feMergeNode in="SourceGraphic" />
        </feMerge>
      </filter>
    </defs>

    <!-- Static circuit edges -->
    {#each EDGES as [a, b], i}
      <line
        x1={pct(NODES[a].cx, 'x')} y1={pct(NODES[a].cy, 'y')}
        x2={pct(NODES[b].cx, 'x')} y2={pct(NODES[b].cy, 'y')}
        stroke={i % 5 === 0 ? 'rgba(255,0,60,0.25)' : 'rgba(0,240,255,0.18)'}
        stroke-width="0.15"
      />
    {/each}

    <!-- Node dots -->
    {#each NODES as node, i}
      <circle
        cx={pct(node.cx, 'x')} cy={pct(node.cy, 'y')}
        r="0.35"
        fill={i % 4 === 0 ? '#FF003C' : '#00F0FF'}
        opacity="0.6"
        filter={i % 4 === 0 ? 'url(#glow-mag)' : 'url(#glow-cyan)'}
      />
    {/each}

    <!-- Animated data packets travelling along edges -->
    {#each PACKETS as pkt}
      {@const nodeA = NODES[pkt.edge[0]]}
      {@const nodeB = NODES[pkt.edge[1]]}
      <circle r="0.5" fill={pkt.color} filter={pkt.color === '#00F0FF' ? 'url(#glow-cyan)' : 'url(#glow-mag)'}>
        <animateMotion
          dur="{pkt.dur}s"
          begin="{pkt.delay}s"
          repeatCount="indefinite"
          path="M {nodeA.cx},{nodeA.cy} L {nodeB.cx},{nodeB.cy}"
        />
        <animate attributeName="opacity" values="0;1;1;0" dur="{pkt.dur}s" begin="{pkt.delay}s" repeatCount="indefinite" />
      </circle>
    {/each}
  </svg>

  <!-- Radial vignette to focus the page centre -->
  <div
    class="absolute inset-0"
    style="background: radial-gradient(ellipse at 50% 50%, transparent 40%, #0B0E14 90%);"
  ></div>
</div>
