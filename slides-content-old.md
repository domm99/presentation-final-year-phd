---
theme: default
title: Self-Organizing Federated Learning in Collective Adaptive Systems
titleTemplate: '%s'
colorSchema: light
routerMode: hash
mdc: true
selectable: true
layout: default
class: first-slide
transition: slide-left
defaults:
  layout: default
  transition: slide-left
---
<div class="cover-center-shell">
  <p class="cover-kicker">ACSOS 2026 - Doctoral Symposium </p>
  <h1 class="cover-paper-title">Self-Organizing Federated Learning</h1>
  <h2 class="cover-subtitle">in Collective Adaptive Systems</h2>
  <div class="cover-rule" />
  <div class="cover-meta-row">
    <div class="cover-mini-meta">Davide Domini</div>
  </div>
  <div class="cover-affiliations">
    <BaseImg src="images/disi.svg" alt="Alma Mater Studiorum — Università di Bologna" class="affiliation-logo logo-unibo" />
    <span class="affiliation-divider" aria-hidden="true" />
  </div>
</div>

---
layout: default
class: stage-slide
---

<div class="slide-shell">

# Collective Adaptive Systems

- **Large-scale ensembles** of situated, heterogeneous devices that interact *locally* and adapt over time.
- No global coordinator — system-level behaviour **emerges** from peer-to-peer interactions.
- Examples: smart cities, IoT sensor networks, fleets of autonomous robots.

<div style="display: grid; grid-template-columns: repeat(3, 1fr); gap: 1.4rem; margin-top: 0.7rem;">
  <div v-click style="border-top: 2px solid var(--deck-teal); padding-top: 0.45rem;">
    <div style="font-weight: 700; color: var(--deck-teal);">Decentralised</div>
    <div style="font-size: 0.82rem; color: var(--deck-muted); line-height: 1.35;">Each device observes only a partial, local view of the environment.</div>
  </div>
  <div v-click style="border-top: 2px solid var(--deck-green); padding-top: 0.45rem;">
    <div style="font-weight: 700; color: var(--deck-green);">Spatially non-IID</div>
    <div style="font-size: 0.82rem; color: var(--deck-muted); line-height: 1.35;">Proximity makes nearby devices share similar — but globally heterogeneous — data.</div>
  </div>
  <div v-click style="border-top: 2px solid var(--deck-orange); padding-top: 0.45rem;">
    <div style="font-weight: 700; color: var(--deck-orange);">Dynamic & mobile</div>
    <div style="font-size: 0.82rem; color: var(--deck-muted); line-height: 1.35;">Devices join, leave, and move — topology and distributions shift continuously.</div>
  </div>
</div>

</div>

---
layout: default
class: stage-slide
---

<div class="slide-shell">

# The problem: standard FL was not built for this

<div class="comparison-grid">
  <div class="comparison-card">
    <div class="card-title">Central aggregator</div>
    <div class="card-text">A fixed coordinator is a single point of failure — incompatible with a fully decentralised CAS.</div>
  </div>
  <div class="comparison-card">
    <div class="card-title">IID assumption</div>
    <div class="card-text">Spatial locality creates strong non-IID distributions: FedAvg accuracy degrades significantly.</div>
  </div>
  <div class="comparison-card">
    <div class="card-title">Static topology</div>
    <div class="card-text">Mobility and churn break fixed federation structures — federations must reorganise over time.</div>
  </div>
</div>

<p v-click style="margin-top: 1rem; text-align: center; font-size: 0.95rem; color: var(--deck-muted);">
  <span class="mark-teal">Heterogeneity</span>, <span class="mark-orange">mobility</span>, and <span class="mark-green">privacy</span> are not edge cases in CAS — they are the norm.
</p>

</div>

---
layout: default
class: stage-slide
---

<div class="slide-shell">

# Research objective

<div style="text-align: center; font-size: 1.12rem; font-weight: 600; color: var(--deck-teal); margin: 1.1rem 0 1.3rem; line-height: 1.5;">
  How can learning <em>emerge</em> in a CAS from local device interactions,<br>
  adapting to non-IID data, mobility, and resource constraints?
</div>

- Combine <span class="mark-teal">decentralised FL</span> with <span class="mark-orange">self-organising coordination</span> through **Aggregate Computing (AC)**.
- Federation structure is **not fixed a priori** — it emerges from spatial proximity and model similarity.
- AC provides a coordination paradigm to express FL processes as <span class="mark-green">collective behaviours</span> at the system level.

</div>

---
layout: default
class: viz-slide
---

<div class="slide-shell">

# Approach: self-organising federations

<div class="split-grid">

<div>

<ul>
  <li><strong>From fixed partitions to adaptive regions:</strong>
    <ul>
      <li v-click="1"><strong style="color: var(--deck-teal);">1. Neighbour sensing</strong> — devices exchange lightweight model summaries with nearby peers.</li>
      <li v-click="2"><strong style="color: var(--deck-green);">2. Cluster formation</strong> — proximity + model similarity drive leader election and region boundaries.</li>
      <li v-click="3"><strong style="color: var(--deck-orange);">3. Federated rounds</strong> — AC field operations aggregate updates within each adaptive region.</li>
    </ul>
  </li>
</ul>

</div>

<div class="visual-box">
  <!-- placeholder: spatial map of devices forming coloured FL regions via AC fields -->
  <div style="display:flex;align-items:center;justify-content:center;height:100%;color:var(--deck-muted);font-size:0.85rem;text-align:center;padding:1rem;">
    [Diagram: devices on a 2-D map<br>self-organising into adaptive<br>FL regions via AC fields]
  </div>
</div>

</div>

</div>

---
layout: default
class: stage-slide
---

<div class="slide-shell">

# Contributions

<div style="display: grid; grid-template-columns: repeat(2, 1fr); gap: 1.2rem; margin-top: 0.6rem;">
  <div v-click style="border-top: 2px solid var(--deck-teal); padding-top: 0.45rem;">
    <div style="font-weight: 700; color: var(--deck-teal);">FBFL</div>
    <div style="font-size: 0.82rem; color: var(--deck-muted); line-height: 1.35;">Field-Based FL — federation structure emerges from AC field computations, handling spatial non-IID data.</div>
  </div>
  <div v-click style="border-top: 2px solid var(--deck-green); padding-top: 0.45rem;">
    <div style="font-weight: 700; color: var(--deck-green);">RND clustering</div>
    <div style="font-size: 0.82rem; color: var(--deck-muted); line-height: 1.35;">Random Network Distillation as a cheap novelty signal to discover which devices share a similar data distribution.</div>
  </div>
  <div v-click style="border-top: 2px solid var(--deck-orange); padding-top: 0.45rem;">
    <div style="font-weight: 700; color: var(--deck-orange);">C²FL</div>
    <div style="font-size: 0.82rem; color: var(--deck-muted); line-height: 1.35;">Clustered Continual FL — preserves accuracy under spatial and temporal data drift caused by device mobility.</div>
  </div>
  <div v-click style="border-top: 2px solid var(--deck-teal); padding-top: 0.45rem;">
    <div style="font-weight: 700; color: var(--deck-teal);">Tools & benchmarks</div>
    <div style="font-size: 0.82rem; color: var(--deck-muted); line-height: 1.35;"><strong>ProFed</strong> — proximity-based non-IID benchmark · <strong>Phyelds</strong> — Python AC framework · <strong>SparseFed</strong> — NN sparsification for resource-constrained devices.</div>
  </div>
</div>

</div>

---
layout: default
class: viz-slide
---

<div class="slide-shell">

# Results

<div class="split-grid">

<div>

<v-clicks>

- **Accuracy** — proximity-aware federations match centralised FL under IID; outperform FedAvg, FedProx, Scaffold, and clustered baselines under spatial non-IID (MNIST, CIFAR-100, UTKFace).
- **Robustness** — self-organising structure survives aggregator failures and maintains stable federations under device mobility.
- **Efficiency** — RND signals + NN sparsification reduce communication and compute costs while preserving competitive accuracy.

</v-clicks>

</div>

<div class="visual-box">
  <!-- placeholder: accuracy comparison bar chart across methods and datasets -->
  <div style="display:flex;align-items:center;justify-content:center;height:100%;color:var(--deck-muted);font-size:0.85rem;text-align:center;padding:1rem;">
    [Chart: accuracy vs baselines<br>on CIFAR-100 / UTKFace<br>under spatial non-IID splits]
  </div>
</div>

</div>

</div>

---
layout: default
class: end-slide
transition: fade
---

<div class="slide-shell">

# Takeaways & next steps

<div style="display: grid; grid-template-columns: repeat(3, 1fr); gap: 1.4rem; margin-top: 0.7rem;">
  <div style="border-top: 2px solid var(--deck-teal); padding-top: 0.45rem;">
    <div style="font-weight: 700; color: var(--deck-teal);">CAS need a new FL</div>
    <div style="font-size: 0.82rem; color: var(--deck-muted); line-height: 1.35;">Self-organising, decentralised — not just a distributed version of standard FL.</div>
  </div>
  <div style="border-top: 2px solid var(--deck-green); padding-top: 0.45rem;">
    <div style="font-weight: 700; color: var(--deck-green);">AC is the bridge</div>
    <div style="font-size: 0.82rem; color: var(--deck-muted); line-height: 1.35;">Aggregate Computing lets federation structure emerge from local device interactions.</div>
  </div>
  <div style="border-top: 2px solid var(--deck-orange); padding-top: 0.45rem;">
    <div style="font-weight: 700; color: var(--deck-orange);">Promising results</div>
    <div style="font-size: 0.82rem; color: var(--deck-muted); line-height: 1.35;">Better accuracy + robustness under non-IID data, mobility, and resource constraints.</div>
  </div>
</div>

<p class="closing-line">
  Next: <span class="mark-teal">real-world spatial benchmarks</span> · <span class="mark-orange">FL as fully self-adaptive architecture</span> · <span class="mark-green">cheaper similarity proxies</span>
</p>

</div>

---
layout: default
class: end-slide
---

<div class="slide-shell">

<div class="thanks-grid" style="grid-template-columns: 1fr;">

<div class="thanks-left">
  <div class="thanks-title">Thank You!</div>
  <Logo text="Self-Org FL" style="margin-bottom: 0.2rem;" />

  <div class="thanks-subtitle">
    Self-Organizing Federated Learning in Collective Adaptive Systems
  </div>
  <div class="thanks-qr-row">
    <QrCard title="ProFed benchmark" url="https://openresearchsoftware.metajnl.com/articles/10.5334/jors.624" :size="5.8" />
    <QrCard title="Phyelds" url="https://arxiv.org/abs/2603.29999" :size="5.8" />
  </div>
  <div class="thanks-details">
    <strong>Contact:</strong> davide.domini@unibo.it · Dept. of Computer Science and Engineering · Università di Bologna
  </div>
</div>

</div>

</div>

---
layout: default
class: stage-slide
---

<div class="slide-shell">

# References

<References />

</div>
