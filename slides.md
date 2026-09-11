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
transition: fade
defaults:
  layout: default
  transition: slide-left
---

<div class="cover-center-shell">
  <p class="cover-kicker">ACSOS 2026 · Doctoral Symposium · Cesena</p>
  <h1 class="cover-paper-title">Self-Organizing Federated Learning</h1>
  <h2 class="cover-subtitle">in Collective Adaptive Systems</h2>
  <div class="cover-rule" />
  <div class="cover-meta-row">
    <div class="cover-mini-meta"><strong style="color: var(--deck-orange);">Davide Domini</strong></div>
  </div>
  <div class="cover-affiliations">
    <BaseImg src="images/disi.svg" alt="Alma Mater Studiorum — Università di Bologna" class="affiliation-logo logo-unibo" />
  </div>
</div>

---
layout: default
class: viz-slide
---

<div class="slide-shell">

# Collective Adaptive Systems

<div class="split-grid">

<div>

- **Large-scale ensembles** of situated, heterogeneous devices that interact *locally* and adapt over time.
- No global coordinator — system-level behavior **emerges** from peer-to-peer interactions.
- Examples: smart cities, IoT fleets, swarms of autonomous robots.

<div style="display: grid; grid-template-columns: 1fr; gap: 0.75rem; margin-top: 0.6rem;">
  <div v-click style="border-top: 2px solid var(--deck-teal); padding-top: 0.4rem;">
    <div style="font-weight: 700; color: var(--deck-teal); font-size: 0.88rem;">Decentralized</div>
    <div style="font-size: 0.78rem; color: var(--deck-muted); line-height: 1.3;">Each device observes only a partial, local view of the environment.</div>
  </div>
  <div v-click style="border-top: 2px solid var(--deck-green); padding-top: 0.4rem;">
    <div style="font-weight: 700; color: var(--deck-green); font-size: 0.88rem;">Spatially non-IID</div>
    <div style="font-size: 0.78rem; color: var(--deck-muted); line-height: 1.3;">Proximity makes nearby devices share similar — but globally heterogeneous — data.</div>
  </div>
  <div v-click style="border-top: 2px solid var(--deck-orange); padding-top: 0.4rem;">
    <div style="font-weight: 700; color: var(--deck-orange); font-size: 0.88rem;">Dynamic & mobile</div>
    <div style="font-size: 0.78rem; color: var(--deck-muted); line-height: 1.3;">Devices join, leave, and move — topology and distributions shift continuously.</div>
  </div>
</div>

</div>

<div class="visual-box">
  <CasDomain />
</div>

</div>

</div>

---
layout: default
class: stage-slide top-slide
clicks: 3
---

<div class="slide-shell">

# Federated learning — standard paradigm

> a distributed learning paradigm where devices train a shared model collaboratively without sharing their raw data. <Cite n="1" />

<FederatedLearning :click="$clicks" />
<div v-click="1" class="click-marker" /><div v-click="2" class="click-marker" /><div v-click="3" class="click-marker" />

<Cites refs="1" />

</div>

---
layout: default
class: stage-slide
---

<div class="slide-shell">

# Standard FL was not built for CAS

<div class="comparison-grid">
  <div class="comparison-card">
    <div class="card-title">Central aggregator</div>
    <div class="card-text">A fixed coordinator is a single point of failure — incompatible with a fully decentralized CAS.</div>
  </div>
  <div class="comparison-card">
    <div class="card-title">IID assumption</div>
    <div class="card-text">Spatial locality creates strong non-IID distributions — FedAvg accuracy degrades significantly.</div>
  </div>
  <div class="comparison-card">
    <div class="card-title">Dynamic participation</div>
    <div class="card-text">Mobility and churn break fixed federation structures — federations must reorganize over time.</div>
  </div>
</div>

<p v-click style="margin-top: 1rem; text-align: center; font-size: 0.95rem; color: var(--deck-muted);">
  <span class="mark-teal">Heterogeneity</span>, <span class="mark-orange">mobility</span>, and <span class="mark-green">dynamic participation</span> are not edge cases in CAS — they define it.
</p>

</div>

---
layout: default
class: stage-slide
---

<div class="slide-shell">

# Research objective

<div style="text-align: center; font-size: 1.1rem; font-weight: 600; color: var(--deck-teal); margin: 1.1rem 0 1.3rem; line-height: 1.55; border-left: 3px solid var(--deck-teal); padding-left: 1rem; text-align: left;">
  How can learning <em>emerge</em> in a CAS from local device interactions,<br>
  adapting to non-IID data, mobility, and resource constraints — without a central coordinator?
</div>

- Combine <span class="mark-teal">decentralized FL</span> with <span class="mark-orange">self-organizing coordination</span> via **Aggregate Computing (AC)**.
- Federation structure is **not fixed a priori** — it emerges from spatial proximity and model similarity.
- AC expresses FL processes as <span class="mark-green">collective behaviors</span>: leader election, neighborhood formation, aggregation.

</div>

---
layout: default
class: viz-slide
clicks: 4
---

<div class="slide-shell">

# Self-organizing federations

<FederationViz :click="$clicks" />

</div>

---
layout: default
class: stage-slide
---

<div class="slide-shell">

# Contributions

<div style="display: grid; grid-template-columns: repeat(2, 1fr); gap: 1.0rem; margin-top: 0.1rem;">
  <div v-click style="border-top: 2px solid var(--deck-teal); padding-top: 0.45rem;">
    <div style="font-weight: 700; color: var(--deck-teal);">FBFL <Cite n="9,4" /></div>
    <div style="font-size: 0.82rem; color: var(--deck-muted); line-height: 1.35;">Field-Based Federated Learning — federation structure emerges from AC field computations, handling spatial non-IID data.</div>
  </div>
  <div v-click style="border-top: 2px solid var(--deck-green); padding-top: 0.45rem;">
    <div style="font-weight: 700; color: var(--deck-green);">RND clustering <Cite n="10" /></div>
    <div style="font-size: 0.82rem; color: var(--deck-muted); line-height: 1.35;">Random Network Distillation as a cheap novelty signal to discover which devices share a similar data distribution.</div>
  </div>
  <div v-click style="border-top: 2px solid #1e3a5f; padding-top: 0.45rem;">
    <div style="font-weight: 700; color: #1e3a5f;">C²FL <Cite n="11" /></div>
    <div style="font-size: 0.82rem; color: var(--deck-muted); line-height: 1.35;">Clustered Continual FL — preserves accuracy under spatial and temporal data drift caused by device mobility.</div>
  </div>
  <div v-click style="border-top: 2px solid var(--deck-teal); padding-top: 0.45rem;">
    <div style="font-weight: 700; color: var(--deck-teal);">Tools & benchmarks <Cite n="12,7" /></div>
    <div style="font-size: 0.82rem; color: var(--deck-muted); line-height: 1.35;"><strong>ProFed</strong> — proximity-based non-IID benchmark &nbsp;·&nbsp; <strong>Phyelds</strong> — Python AC framework.</div>
  </div>
</div>

<Cites refs="4,7,9,10,11,12" />

</div>

---
layout: default
class: stage-slide
---

<div class="slide-shell">

# Results

<div style="display: grid; grid-template-columns: repeat(3, 1fr); gap: 1.4rem; margin-top: 0.7rem;">
  <div style="border-top: 2px solid var(--deck-teal); padding-top: 0.45rem;">
    <div style="font-weight: 700; color: var(--deck-teal);">Accuracy</div>
    <div style="font-size: 0.82rem; color: var(--deck-muted); line-height: 1.35;">Self-organizing federations match centralized FL under IID; outperform FedAvg, FedProx, Scaffold, and clustered baselines under spatial non-IID (MNIST, CIFAR-100, UTKFace).</div>
  </div>
  <div style="border-top: 2px solid var(--deck-green); padding-top: 0.45rem;">
    <div style="font-weight: 700; color: var(--deck-green);">Robustness</div>
    <div style="font-size: 0.82rem; color: var(--deck-muted); line-height: 1.35;">Self-organizing structure survives aggregator failures and maintains stable federations under device mobility.</div>
  </div>
  <div style="border-top: 2px solid var(--deck-orange); padding-top: 0.45rem;">
    <div style="font-weight: 700; color: var(--deck-orange);">Efficiency</div>
    <div style="font-size: 0.82rem; color: var(--deck-muted); line-height: 1.35;">RND similarity signals reduce communication and compute costs while preserving competitive accuracy.</div>
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
    <div style="font-size: 0.82rem; color: var(--deck-muted); line-height: 1.35;">Self-organizing, decentralized — not just a distributed version of standard FL.</div>
  </div>
  <div style="border-top: 2px solid var(--deck-green); padding-top: 0.45rem;">
    <div style="font-weight: 700; color: var(--deck-green);">AC is the bridge</div>
    <div style="font-size: 0.82rem; color: var(--deck-muted); line-height: 1.35;">Aggregate Computing lets federation structure emerge from local device interactions.</div>
  </div>
  <div style="border-top: 2px solid var(--deck-orange); padding-top: 0.45rem;">
    <div style="font-weight: 700; color: var(--deck-orange);">Results are promising</div>
    <div style="font-size: 0.82rem; color: var(--deck-muted); line-height: 1.35;">Better accuracy + robustness under non-IID data, mobility, and resource constraints.</div>
  </div>
</div>

<p class="closing-line">
  Next: <span class="mark-teal">real-world spatial benchmarks</span> · <span class="mark-orange">FL as fully self-adaptive architecture</span> · <span class="mark-green">cheaper similarity proxies</span>
</p>

</div>


---
layout: default
class: stage-slide
---

<div class="slide-shell">

# References

<References />

</div>
