<script setup lang="ts">
import { computed } from 'vue'
import MathTex from './MathTex.vue'

const props = defineProps<{ click?: number }>()
const phase = computed(() => Math.min(Math.max(props.click ?? 0, 0) + 1, 4))

const VB = { w: 620, h: 370 }
const R   = 25
const LW  = 90
const LH  = 48

const leaders = [
  { id: 'L1', x: 155, y: 185, zone: 'teal',   label: 'L₁', peerId: 'd7', bx:  78, by:  0  },
  { id: 'L2', x: 400, y: 72,  zone: 'orange', label: 'L₂', peerId: 'd8', bx:   0, by: -44 },
  { id: 'L3', x: 420, y: 292, zone: 'green',  label: 'L₃', peerId: 'd9', bx:   0, by:  50 },
]

const members = [
  { id: 'd1', x: 60,  y: 55,  lid: 'L1', zone: 'teal'   },
  { id: 'd2', x: 60,  y: 308, lid: 'L1', zone: 'teal'   },
  { id: 'd3', x: 255, y: 52,  lid: 'L2', zone: 'orange' },
  { id: 'd4', x: 560, y: 52,  lid: 'L2', zone: 'orange' },
  { id: 'd5', x: 278, y: 318, lid: 'L3', zone: 'green'  },
  { id: 'd6', x: 570, y: 318, lid: 'L3', zone: 'green'  },
]

function tRect(ux: number, uy: number) {
  const ax = Math.abs(ux), ay = Math.abs(uy)
  return Math.min(ax > 1e-4 ? (LW / 2 + 6) / ax : 1e9,
                  ay > 1e-4 ? (LH / 2 + 6) / ay : 1e9)
}

// Intra edges: gap at leader side depends on phase (circle in ph1, rect in ph2+)
const intraEdges = computed(() =>
  members.map(m => {
    const l = leaders.find(l => l.id === m.lid)!
    const dx = l.x - m.x, dy = l.y - m.y
    const len = Math.hypot(dx, dy)
    const ux = dx / len, uy = dy / len
    const gapM = R + 9
    const gapL = R + 9
    const x1 = m.x + ux * gapM,  y1 = m.y + uy * gapM
    const x2 = l.x - ux * gapL,  y2 = l.y - uy * gapL
    return { id: `${m.id}${l.id}`, x1, y1, x2, y2,
             mx: (x1+x2)/2, my: (y1+y2)/2,
             dx: x1+0.30*(x2-x1), dy: y1+0.30*(y2-y1),
             zone: m.zone, lid: l.id }
  })
)

// Cross edges: leader↔leader (sensing phase 1 only)
const crossEdges = computed(() => {
  const out: any[] = []
  for (let i = 0; i < leaders.length; i++)
    for (let j = i+1; j < leaders.length; j++) {
      const a = leaders[i], b = leaders[j]
      const dx = b.x - a.x, dy = b.y - a.y
      const len = Math.hypot(dx, dy)
      const ux = dx/len, uy = dy/len
      const gapA = R + 9
      const gapB = R + 9
      const x1 = a.x + ux*gapA, y1 = a.y + uy*gapA
      const x2 = b.x - ux*gapB, y2 = b.y - uy*gapB
      out.push({ id: `${a.id}${b.id}`, x1, y1, x2, y2, mx:(x1+x2)/2, my:(y1+y2)/2 })
    }
  return out
})

const zones = [
  { zone:'teal',   cx: 105, cy:185, rx:105, ry:165 },
  { zone:'orange', cx:408,  cy: 65, rx:178, ry: 50 },
  { zone:'green',  cx:424,  cy:306, rx:171, ry: 44 },
]

const steps = [
  { n:1, strong:'Neighbor similarities',
    small:'devices compute pairwise model similarity σᵢⱼ with nearby peers' },
  { n:2, strong:'Leader election',
    small:'proximity + model similarity drive cluster formation' },
  { n:3, strong:'Local training',
    small:"a few epochs on each device's private data" },
  { n:4, strong:'Aggregate (Δᵢ)',
    small:'weight deltas flow to cluster leader; FedAvg runs locally' },
]
</script>

<template>
  <div class="fv-explainer">
    <svg class="fv-scene" :class="`is-phase-${phase}`"
         :viewBox="`0 0 ${VB.w} ${VB.h}`" role="img">
      <defs>
        <marker v-for="z in ['teal','orange','green']" :key="z"
          :id="`fvh-${z}`" viewBox="0 0 10 10" refX="9.5" refY="5"
          markerWidth="5" markerHeight="5" orient="auto-start-reverse" markerUnits="strokeWidth">
          <path d="M0 0 L10 5 L0 10 Z" :class="`fvfill-${z}`"/>
        </marker>
      </defs>

      <!-- Zone blobs (phase 2+) -->
      <ellipse v-for="z in zones" :key="z.zone"
        :cx="z.cx" :cy="z.cy" :rx="z.rx" :ry="z.ry"
        :class="['fv-blob', `blob-${z.zone}`]"/>

      <!-- Cross edges (sensing only, phase 1) -->
      <g class="cross-group">
        <line v-for="e in crossEdges" :key="`cr-${e.id}`"
          :x1="e.x1" :y1="e.y1" :x2="e.x2" :y2="e.y2" class="cross-rail"/>
        <line v-for="e in crossEdges" :key="`cl-${e.id}`"
          :x1="e.x1" :y1="e.y1" :x2="e.x2" :y2="e.y2" class="cross-live"/>
        <g v-for="e in crossEdges" :key="`cs-${e.id}`"
          :transform="`translate(${e.mx},${e.my})`" class="sim-chip">
          <rect x="-22" y="-12" width="44" height="24" rx="3"/>
          <foreignObject x="-22" y="-12" width="44" height="24">
            <div class="chip-inner"><MathTex math="\sigma_{ij}"/></div>
          </foreignObject>
        </g>
      </g>

      <!-- Intra edges -->
      <line v-for="e in intraEdges" :key="`ir-${e.id}`"
        :x1="e.x1" :y1="e.y1" :x2="e.x2" :y2="e.y2" class="edge-rail"/>
      <line v-for="e in intraEdges" :key="`il-${e.id}`"
        :x1="e.x1" :y1="e.y1" :x2="e.x2" :y2="e.y2"
        :class="['edge-live', `tone-${e.zone}`]"
        :marker-end="phase===4 ? `url(#fvh-${e.zone})` : undefined"/>

      <!-- Δᵢ chips (phase 4) -->
      <g v-if="phase===4" v-for="e in intraEdges" :key="`dc-${e.id}`"
        :transform="`translate(${e.dx},${e.dy})`"
        :class="['delta-chip', `chip-${e.zone}`]">
        <rect x="-24" y="-13" width="48" height="26" rx="4"/>
        <foreignObject x="-24" y="-13" width="48" height="26">
          <div class="chip-inner"><MathTex math="\Delta_i"/></div>
        </foreignObject>
      </g>

      <!-- Sim chips on intra edges (phase 1 only) -->
      <g v-for="e in intraEdges" :key="`is-${e.id}`"
        :transform="`translate(${e.mx},${e.my})`"
        :class="['intra-sim-chip', `chip-${e.zone}`]">
        <rect x="-22" y="-12" width="44" height="24" rx="3"/>
        <foreignObject x="-22" y="-12" width="44" height="24">
          <div class="chip-inner"><MathTex math="\sigma_{ij}"/></div>
        </foreignObject>
      </g>

      <!-- Member nodes -->
      <g v-for="m in members" :key="m.id" :class="['fv-client', `mc-${m.zone}`]">
        <circle class="client-halo" :cx="m.x" :cy="m.y" :r="R+8"/>
        <circle class="client-ring" :cx="m.x" :cy="m.y" :r="R"/>
        <text class="client-id" :x="m.x" :y="m.y+0.5"
              dominant-baseline="middle" text-anchor="middle">{{ m.id }}</text>
        <g class="client-badge" :transform="`translate(${m.x},${m.y-R-15})`">
          <rect x="-32" y="-9.5" width="64" height="19" rx="3"/>
          <text x="0" y="0.5" dominant-baseline="middle" text-anchor="middle">training…</text>
        </g>
      </g>

      <!-- Leader nodes -->
      <g v-for="l in leaders" :key="l.id" :class="['fv-leader', `lc-${l.zone}`]">
        <!-- Phase 1: leader appears as a plain circle (same as members) -->
        <circle class="leader-circle" :cx="l.x" :cy="l.y" :r="R"/>
        <text class="leader-circle-id" :x="l.x" :y="l.y+0.5"
              dominant-baseline="middle" text-anchor="middle">{{ l.peerId }}</text>

        <!-- Phase 4: FedAvg badge -->
        <g class="leader-badge" :transform="`translate(${l.x + l.bx},${l.y + l.by})`">
          <rect x="-46" y="-12" width="92" height="24" rx="3"/>
          <foreignObject x="-46" y="-12" width="92" height="24">
            <div class="badge-inner">FedAvg → <MathTex math="\omega_k'"/></div>
          </foreignObject>
        </g>
      </g>

    </svg>

    <ol class="fv-steps">
      <li v-for="s in steps" :key="s.n"
          class="fv-step" :class="{ active: phase===s.n, done: phase>s.n }">
        <span class="num">{{ s.n }}</span>
        <span class="txt">
          <strong>{{ s.strong }}</strong>
          <small>{{ s.small }}</small>
        </span>
      </li>
    </ol>
  </div>
</template>

<style scoped>
.fv-explainer {
  display: grid; grid-template-columns: 1.15fr 0.85fr;
  gap: 1.2rem; align-items: stretch; margin-top: 0.4rem;
}
.fv-scene {
  width: 100%; height: auto;
  border: 1px solid rgba(16,32,43,0.12);
  background: linear-gradient(135deg,#f3f8f9,#ffffff);
}
.fvfill-teal   { fill: var(--deck-teal);   }
.fvfill-orange { fill: var(--deck-orange); }
.fvfill-green  { fill: var(--deck-green);  }

/* Zone blobs */
.fv-blob { opacity:0; transition: opacity 500ms ease; }
.blob-teal   { fill: var(--deck-teal);   }
.blob-orange { fill: var(--deck-orange); }
.blob-green  { fill: var(--deck-green);  }
.is-phase-2 .fv-blob,
.is-phase-3 .fv-blob,
.is-phase-4 .fv-blob { opacity: 0.08; }

/* Cross edges */
.cross-rail { stroke:rgba(15,76,92,0.18); stroke-width:1.2; stroke-dasharray:3 5; }
.cross-live { stroke:transparent; stroke-width:2; stroke-dasharray:10 7; stroke-linecap:round; }
.sim-chip { opacity:0; }
.sim-chip rect { fill:var(--deck-muted,#64748b); stroke:#f6fafb; stroke-width:3; paint-order:stroke; }
.sim-chip text { fill:#fff; font-family:var(--deck-font-mono); font-size:11px; font-weight:600; }
.intra-sim-chip { opacity:0; transition: opacity 300ms; }
.intra-sim-chip rect { stroke:#f6fafb; stroke-width:3; paint-order:stroke; }
.chip-teal   .intra-sim-chip rect,
.intra-sim-chip.chip-teal   rect { fill:var(--deck-teal);   }
.intra-sim-chip.chip-orange rect { fill:var(--deck-orange); }
.intra-sim-chip.chip-green  rect { fill:var(--deck-green);  }
.is-phase-1 .intra-sim-chip { opacity:1; }

.is-phase-1 .cross-live { stroke:var(--deck-muted,#64748b); animation: fv-flow 1s linear infinite; }
.is-phase-1 .sim-chip { opacity:1; }
.is-phase-2 .cross-group,
.is-phase-3 .cross-group,
.is-phase-4 .cross-group { opacity:0; pointer-events:none; }

/* Intra edges */
.edge-rail { stroke:rgba(15,76,92,0.15); stroke-width:1.5; stroke-dasharray:3 5; }
.edge-live { stroke:transparent; stroke-width:2.4; stroke-dasharray:11 8; stroke-linecap:round; }
.is-phase-1 .edge-live.tone-teal   { stroke:var(--deck-teal);   animation:fv-flow 1.1s linear infinite; }
.is-phase-1 .edge-live.tone-orange { stroke:var(--deck-orange); animation:fv-flow 1.1s linear infinite; }
.is-phase-1 .edge-live.tone-green  { stroke:var(--deck-green);  animation:fv-flow 1.1s linear infinite; }
.is-phase-4 .edge-live.tone-teal   { stroke:var(--deck-teal);   animation:fv-flow-up 1.1s linear infinite; }
.is-phase-4 .edge-live.tone-orange { stroke:var(--deck-orange); animation:fv-flow-up 1.1s linear infinite; }
.is-phase-4 .edge-live.tone-green  { stroke:var(--deck-green);  animation:fv-flow-up 1.1s linear infinite; }

@keyframes fv-flow    { to { stroke-dashoffset: -17; } }
@keyframes fv-flow-up { to { stroke-dashoffset:  17; } }

/* Δᵢ chips */
.delta-chip rect { stroke:#f6fafb; stroke-width:3; paint-order:stroke; }
.chip-teal   rect { fill:var(--deck-teal);   }
.chip-orange rect { fill:var(--deck-orange); }
.chip-green  rect { fill:var(--deck-green);  }
.chip-inner { display:flex; align-items:center; justify-content:center; height:100%; color:#fff; }
.chip-inner :deep(.math-tex) { font-size:14px !important; }

/* Member nodes */
.client-halo { fill:none; stroke:none; }
.client-ring { fill:#fff; stroke-width:2.2; transition:stroke 400ms ease; }
.client-id   { font-family:var(--deck-font-mono); font-size:14px; font-weight:700; transition:fill 400ms ease; }
.client-sub  { fill:var(--deck-muted); font-family:var(--deck-font-mono); font-size:10px; }
.client-badge { opacity:0; }
.client-badge rect { stroke:#f6fafb; stroke-width:3; paint-order:stroke; }
.client-badge text { fill:#fff; font-family:var(--deck-font-mono); font-size:10px; font-weight:600; }

.mc-teal   .client-ring { stroke:var(--deck-teal);   }
.mc-teal   .client-id   { fill:  var(--deck-teal);   }
.mc-orange .client-ring { stroke:var(--deck-orange); }
.mc-orange .client-id   { fill:  var(--deck-orange); }
.mc-green  .client-ring { stroke:var(--deck-green);  }
.mc-green  .client-id   { fill:  var(--deck-green);  }

.is-phase-2 .client-halo { fill:none; stroke-width:1.4; stroke-dasharray:3 4;
  animation:fv-spin 8s linear infinite; transform-box:fill-box; transform-origin:center; }
.is-phase-2 .mc-teal   .client-halo { stroke:var(--deck-teal);   }
.is-phase-2 .mc-orange .client-halo { stroke:var(--deck-orange); }
.is-phase-2 .mc-green  .client-halo { stroke:var(--deck-green);  }

.is-phase-3 .client-badge { opacity:1; }
.is-phase-3 .mc-teal   .client-badge rect { fill:var(--deck-teal);   }
.is-phase-3 .mc-orange .client-badge rect { fill:var(--deck-orange); }
.is-phase-3 .mc-green  .client-badge rect { fill:var(--deck-green);  }
.is-phase-3 .client-halo { fill:none; stroke-width:1.4; stroke-dasharray:3 4;
  animation:fv-spin 5s linear infinite; transform-box:fill-box; transform-origin:center; }
.is-phase-3 .mc-teal   .client-halo { stroke:var(--deck-teal);   }
.is-phase-3 .mc-orange .client-halo { stroke:var(--deck-orange); }
.is-phase-3 .mc-green  .client-halo { stroke:var(--deck-green);  }

@keyframes fv-spin { to { transform: rotate(360deg); } }

/* Leader nodes */
/* Phase 1: show as plain circle, hide box */
.leader-circle    { fill:#fff; stroke:#8fa9b4; stroke-width:2.2; transition:opacity 400ms; }
.leader-circle-id { fill:#8fa9b4; font-family:var(--deck-font-mono); font-size:14px; font-weight:700; transition:opacity 400ms; }
.lc-teal   .leader-circle    { stroke:var(--deck-teal);   }
.lc-teal   .leader-circle-id { fill:  var(--deck-teal);   }
.lc-orange .leader-circle    { stroke:var(--deck-orange); }
.lc-orange .leader-circle-id { fill:  var(--deck-orange); }
.lc-green  .leader-circle    { stroke:var(--deck-green);  }
.lc-green  .leader-circle-id { fill:  var(--deck-green);  }
.leader-box       { fill:#8fa9b4; transition:fill 400ms ease, opacity 400ms; }
.leader-id        { fill:#fff; font-size:14px; font-weight:700; letter-spacing:.06em; font-family:var(--deck-font-mono); transition:opacity 400ms; }
.leader-sub       { fill:rgba(255,255,255,.75); font-size:10px; font-family:var(--deck-font-mono); transition:opacity 400ms; }
.leader-badge     { opacity:0; }
.leader-badge rect { stroke:#f6fafb; stroke-width:3.5; paint-order:stroke; }
.badge-inner { display:flex; align-items:center; justify-content:center; gap:.25em; height:100%; color:#fff; font-family:var(--deck-font-mono); font-size:10.5px; font-weight:600; }
.badge-inner :deep(.math-tex) { font-size:11px !important; }


/* Phase 2+: circle hidden, box visible + colored */
.is-phase-2 .lc-teal   .leader-circle,
.is-phase-3 .lc-teal   .leader-circle,
.is-phase-4 .lc-teal   .leader-circle { fill:var(--deck-teal);   stroke:var(--deck-teal); }
.is-phase-2 .lc-orange .leader-circle,
.is-phase-3 .lc-orange .leader-circle,
.is-phase-4 .lc-orange .leader-circle { fill:var(--deck-orange); stroke:var(--deck-orange); }
.is-phase-2 .lc-green  .leader-circle,
.is-phase-3 .lc-green  .leader-circle,
.is-phase-4 .lc-green  .leader-circle { fill:var(--deck-green);  stroke:var(--deck-green); }
.is-phase-2 .leader-circle-id,
.is-phase-3 .leader-circle-id,
.is-phase-4 .leader-circle-id { fill:#fff; }



.is-phase-4 .leader-badge { opacity:1; }
.is-phase-4 .lc-teal   .leader-badge rect { fill:var(--deck-teal);   }
.is-phase-4 .lc-orange .leader-badge rect { fill:var(--deck-orange); }
.is-phase-4 .lc-green  .leader-badge rect { fill:var(--deck-green);  }

/* Step list */
.fv-steps { display:grid; grid-template-rows:repeat(4,1fr); gap:.45rem; margin:0; padding:0; list-style:none; }
.fv-step {
  display:grid; grid-template-columns:1.6rem 1fr; gap:.6rem; align-items:center;
  padding:.5rem .65rem; border-left:4px solid #cbd7dc;
  background:rgba(255,255,255,.72); color:var(--deck-muted);
  opacity:.6; transition:300ms ease;
}
.fv-step .num {
  display:grid; place-items:center; width:1.6rem; height:1.6rem; border-radius:50%;
  background:#dbe4e8; font-family:var(--deck-font-mono); font-size:.72rem !important; font-weight:700;
}
.fv-step .txt { min-width:0; }
.fv-step strong { display:block; color:inherit; font-size:.9rem !important; line-height:1.2; }
.fv-step small  { display:block; margin-top:.1rem; font-size:.68rem !important; line-height:1.3; }
.fv-step.active { opacity:1; border-left-color:var(--deck-orange); background:var(--deck-orange-soft); color:#9a5302; }
.fv-step.active .num { background:var(--deck-orange); color:#fff; }
.fv-step.done   { opacity:.9; border-left-color:var(--deck-teal); color:var(--deck-teal); }
.fv-step.done   .num { background:var(--deck-teal); color:#fff; }
</style>
