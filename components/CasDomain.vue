<script setup lang="ts">
const CYCLE = 9

const nodes = [
  // Zone A (teal) — left
  { id: 'n1', cx: 20, cy: 34, zone: 'teal',   d: 0.0 },
  { id: 'n2', cx: 31, cy: 47, zone: 'teal',   d: 0.4 },
  { id: 'n3', cx: 18, cy: 56, zone: 'teal',   d: 0.8 },
  // Zone B (orange) — top-right
  { id: 'n4', cx: 60, cy: 20, zone: 'orange', d: 0.2 },
  { id: 'n5', cx: 74, cy: 28, zone: 'orange', d: 0.6 },
  { id: 'n6', cx: 64, cy: 40, zone: 'orange', d: 1.0 },
  // Zone C (green) — bottom-right
  { id: 'n7', cx: 45, cy: 67, zone: 'green',  d: 0.3 },
  { id: 'n8', cx: 59, cy: 74, zone: 'green',  d: 0.7 },
  { id: 'n9', cx: 71, cy: 64, zone: 'green',  d: 1.1 },
]

const edges = [
  // Intra-zone A
  { x1: 20, y1: 34, x2: 31, y2: 47, zone: 'teal'   },
  { x1: 31, y1: 47, x2: 18, y2: 56, zone: 'teal'   },
  // Intra-zone B
  { x1: 60, y1: 20, x2: 74, y2: 28, zone: 'orange' },
  { x1: 74, y1: 28, x2: 64, y2: 40, zone: 'orange' },
  { x1: 60, y1: 20, x2: 64, y2: 40, zone: 'orange' },
  // Intra-zone C
  { x1: 45, y1: 67, x2: 59, y2: 74, zone: 'green'  },
  { x1: 59, y1: 74, x2: 71, y2: 64, zone: 'green'  },
  // Cross-zone (visible!)
  { x1: 31, y1: 47, x2: 60, y2: 20, zone: 'cross'  },
  { x1: 31, y1: 47, x2: 45, y2: 67, zone: 'cross'  },
  { x1: 64, y1: 40, x2: 59, y2: 74, zone: 'cross'  },
]

const packets = [
  { x1: 20, y1: 34, x2: 31, y2: 47, zone: 'teal',   d: 0.0 },
  { x1: 31, y1: 47, x2: 18, y2: 56, zone: 'teal',   d: 0.5 },
  { x1: 60, y1: 20, x2: 74, y2: 28, zone: 'orange', d: 0.2 },
  { x1: 74, y1: 28, x2: 64, y2: 40, zone: 'orange', d: 0.7 },
  { x1: 45, y1: 67, x2: 59, y2: 74, zone: 'green',  d: 0.1 },
  { x1: 59, y1: 74, x2: 71, y2: 64, zone: 'green',  d: 0.6 },
  // Cross-zone packets
  { x1: 31, y1: 47, x2: 60, y2: 20, zone: 'cross',  d: 0.3 },
  { x1: 31, y1: 47, x2: 45, y2: 67, zone: 'cross',  d: 0.8 },
  { x1: 64, y1: 40, x2: 59, y2: 74, zone: 'cross',  d: 0.5 },
]

const steps = [
  { label: 'Sensing',       n: 1 },
  { label: 'Computing',     n: 2 },
  { label: 'Communicating', n: 3 },
]
</script>

<template>
  <div class="cas-wrap" aria-hidden="true">
    <svg viewBox="0 0 96 88" class="cas-svg" preserveAspectRatio="xMidYMid meet">
      <defs>
        <filter id="glow-f" x="-40%" y="-40%" width="180%" height="180%">
          <feGaussianBlur stdDeviation="1.2" result="blur"/>
          <feMerge><feMergeNode in="blur"/><feMergeNode in="SourceGraphic"/></feMerge>
        </filter>
      </defs>

      <!-- Zone blobs — closer together -->
      <ellipse cx="24" cy="46" rx="15" ry="18" class="zone teal-zone"/>
      <ellipse cx="65" cy="30" rx="16" ry="16" class="zone orange-zone"/>
      <ellipse cx="59" cy="69" rx="19" ry="12" class="zone green-zone"/>

      <!-- Zone labels — near their zones -->
      <text x="7"  y="28" class="zone-lbl teal-lbl">Zone A</text>
      <text x="60" y="11" class="zone-lbl orange-lbl">Zone B</text>
      <text x="72" y="84" class="zone-lbl green-lbl">Zone C</text>

      <!-- Edges -->
      <line v-for="(e,i) in edges" :key="`e${i}`"
        :x1="e.x1" :y1="e.y1" :x2="e.x2" :y2="e.y2"
        :class="['cas-edge', `edge-${e.zone}`]" />

      <!-- Packets -->
      <circle v-for="(p,i) in packets" :key="`p${i}`"
        r="1.4" :cx="p.x1" :cy="p.y1"
        :class="['cas-pkt', `pkt-${p.zone}`]">
        <animate attributeName="cx"
          :values="`${p.x1}; ${p.x1}; ${p.x2}; ${p.x2}`"
          keyTimes="0; 0.667; 0.944; 1"
          :dur="`${CYCLE}s`" :begin="`${p.d}s`" repeatCount="indefinite"/>
        <animate attributeName="cy"
          :values="`${p.y1}; ${p.y1}; ${p.y2}; ${p.y2}`"
          keyTimes="0; 0.667; 0.944; 1"
          :dur="`${CYCLE}s`" :begin="`${p.d}s`" repeatCount="indefinite"/>
        <animate attributeName="opacity"
          values="0; 0; 1; 1; 0"
          keyTimes="0; 0.667; 0.69; 0.944; 1"
          :dur="`${CYCLE}s`" :begin="`${p.d}s`" repeatCount="indefinite"/>
      </circle>

      <!-- Nodes -->
      <g v-for="n in nodes" :key="n.id">
        <circle :cx="n.cx" :cy="n.cy" r="4.5"
          :class="['sense-ring', `sr-${n.zone}`]"
          :style="`animation-delay: ${n.d}s`"/>
        <circle :cx="n.cx" :cy="n.cy" r="4.5"
          :class="['compute-ring', `cr-${n.zone}`]"
          :style="`animation-delay: ${n.d}s`"/>
        <circle :cx="n.cx" :cy="n.cy" r="4.2"
          :class="['node-body', `nb-${n.zone}`]"
          filter="url(#glow-f)"/>
        <circle :cx="n.cx" :cy="n.cy" r="1.5" class="node-eye"/>
      </g>
    </svg>

    <!-- Phase legend -->
    <div class="cas-legend">
      <div v-for="(s, i) in steps" :key="i" class="leg-item" :class="`leg-${i}`">
        <span class="leg-num">{{ s.n }}</span>
        <span class="leg-label">{{ s.label }}</span>
      </div>
      <div class="leg-arrow arr-1">→</div>
      <div class="leg-arrow arr-2">→</div>
    </div>
  </div>
</template>

<style scoped>
.cas-wrap {
  width: 100%; height: 100%;
  display: flex; flex-direction: column;
  align-items: center; justify-content: center;
  gap: 0.4rem;
}
.cas-svg { width: 100%; flex: 1; min-height: 0; }

/* Zones */
.zone { opacity: 0.10; }
.teal-zone   { fill: var(--deck-teal,   #0d9488); }
.orange-zone { fill: var(--deck-orange, #ea7c2b); }
.green-zone  { fill: var(--deck-green,  #16a34a); }

/* Zone labels */
.zone-lbl {
  font-size: 4px; font-family: inherit;
  font-weight: 700; letter-spacing: 0.05em; opacity: 0.6;
}
.teal-lbl   { fill: var(--deck-teal,   #0d9488); }
.orange-lbl { fill: var(--deck-orange, #ea7c2b); }
.green-lbl  { fill: var(--deck-green,  #16a34a); }

/* Edges */
.cas-edge { stroke-width: 0.6; opacity: 0.45; }
.edge-teal   { stroke: var(--deck-teal,   #0d9488); stroke-dasharray: 2.5 2; }
.edge-orange { stroke: var(--deck-orange, #ea7c2b); stroke-dasharray: 2.5 2; }
.edge-green  { stroke: var(--deck-green,  #16a34a); stroke-dasharray: 2.5 2; }
.edge-cross  { stroke: var(--deck-line,   #94a3b8); stroke-dasharray: 1.8 2; opacity: 0.55; }

/* Packets */
.cas-pkt { opacity: 0; }
.pkt-teal   { fill: var(--deck-teal,   #0d9488); }
.pkt-orange { fill: var(--deck-orange, #ea7c2b); }
.pkt-green  { fill: var(--deck-green,  #16a34a); }
.pkt-cross  { fill: var(--deck-line,   #94a3b8); }

/* Sensing ring */
.sense-ring {
  fill: none; stroke-width: 0.7; opacity: 0;
  animation: sense-expand 9s ease-out infinite both;
}
.sr-teal   { stroke: var(--deck-teal,   #0d9488); }
.sr-orange { stroke: var(--deck-orange, #ea7c2b); }
.sr-green  { stroke: var(--deck-green,  #16a34a); }

@keyframes sense-expand {
  0%   { r: 4.5; opacity: 0.85; }
  33%  { r: 12;  opacity: 0; }
  100% { r: 12;  opacity: 0; }
}

/* Compute ring */
.compute-ring {
  fill: none; stroke-width: 1.2; stroke-dasharray: 3 2; opacity: 0;
  animation: compute-breathe 9s ease-in-out infinite both;
}
.cr-teal   { stroke: var(--deck-teal,   #0d9488); }
.cr-orange { stroke: var(--deck-orange, #ea7c2b); }
.cr-green  { stroke: var(--deck-green,  #16a34a); }

@keyframes compute-breathe {
  0%, 33% { r: 4.5; opacity: 0; }
  39%  { r: 5.8; opacity: 0.9; }
  45%  { r: 4.5; opacity: 0.45; }
  52%  { r: 6.2; opacity: 0.9; }
  58%  { r: 4.5; opacity: 0.45; }
  64%  { r: 6.0; opacity: 0.9; }
  66%, 100% { opacity: 0; }
}

/* Node */
.node-body { stroke: white; stroke-width: 0.9; opacity: 0.92; }
.nb-teal   { fill: var(--deck-teal,   #0d9488); }
.nb-orange { fill: var(--deck-orange, #ea7c2b); }
.nb-green  { fill: var(--deck-green,  #16a34a); }
.node-eye  { fill: white; opacity: 0.88; }

/* Legend */
.cas-legend {
  display: flex; align-items: center; gap: 0.4rem;
  padding: 0.25rem 0.7rem;
  background: color-mix(in oklch, var(--deck-ink, #0f1f2b) 5%, transparent);
  border: 1px solid color-mix(in oklch, var(--deck-line, #cbd5e1) 60%, transparent);
  border-radius: 999px; font-size: 0.72rem;
}
.leg-arrow { color: var(--deck-muted, #64748b); font-size: 0.65rem; opacity: 0.5; }
.arr-1 { order: 2; }
.arr-2 { order: 4; }

.leg-item {
  display: flex; align-items: center; gap: 0.3rem;
  opacity: 0.3;
  animation: 9s linear infinite both;
}
.leg-0 { order: 1; animation-name: leg-phase0; }
.leg-1 { order: 3; animation-name: leg-phase1; }
.leg-2 { order: 5; animation-name: leg-phase2; }

.leg-num {
  width: 1.2rem; height: 1.2rem; border-radius: 50%;
  background: var(--deck-line, #e2e8f0);
  color: var(--deck-muted, #64748b);
  display: flex; align-items: center; justify-content: center;
  font-size: 0.62rem; font-weight: 700;
}
.leg-label { font-weight: 500; color: var(--deck-muted, #64748b); }

@keyframes leg-phase0 {
  0%, 28%  { opacity: 1; }
  36%,100% { opacity: 0.28; }
}
@keyframes leg-phase1 {
  0%,  33% { opacity: 0.28; }
  38%, 62% { opacity: 1; }
  68%,100% { opacity: 0.28; }
}
@keyframes leg-phase2 {
  0%,  65% { opacity: 0.28; }
  70%, 97% { opacity: 1; }
  100%     { opacity: 0.28; }
}

@media (prefers-reduced-motion: reduce) {
  .sense-ring, .compute-ring { animation: none; opacity: 0; }
  .cas-pkt { display: none; }
  .leg-item { animation: none; opacity: 0.7; }
}
</style>
