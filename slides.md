---
theme: default
title: Engineering Many-Agent Cooperative Learning in Collective Adaptive Systems
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
  <p class="cover-kicker">DISI · Revisione Annuale Dottorato · 2026</p>
  <h1 class="cover-paper-title">Engineering Many-Agent Cooperative Learning</h1>
  <h2 class="cover-subtitle">in Collective Adaptive Systems</h2>
  <div class="cover-rule" />
  <div class="cover-meta-row">
    <div class="cover-mini-meta"><strong style="color: var(--deck-orange);">Davide Domini</strong></div>
  </div>
  <div class="cover-meta-row" style="margin-top:0.4rem; font-size:0.78rem; color: var(--deck-muted);">
    <div class="cover-mini-meta">Supervisors: Mirko Viroli · Danilo Pianini · Matteo Ferrara</div>
  </div>
  <div class="cover-affiliations">
    <BaseImg src="images/disi.svg" alt="Alma Mater Studiorum — Università di Bologna" class="affiliation-logo logo-unibo" />
  </div>
</div>

---
layout: default
class: section-slide
transition: fade
---

<div class="section-eyebrow">Part I</div>
<h1>Research</h1>

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
class: section-slide
transition: fade
---

<div class="section-eyebrow">Part II</div>
<h1>Contributions &amp; Collaborations</h1>

---
layout: default
class: stage-slide
transition: slide-left
---

<div class="slide-shell">

# Research Contributions

<div style="display:grid; grid-template-columns:175px 1fr 36px; gap:0.55rem 0.9rem; align-items:center; margin-top:0.8rem;">

  <!-- JOURNALS -->
  <div style="text-align:right; font-size:0.88rem; font-weight:600; color:var(--deck-ink); white-space:nowrap;">Journals</div>
  <div style="position:relative; height:42px;">
    <div style="position:absolute;left:calc(3/12*100%);top:0;bottom:0;width:1px;background:#d6e0e6;"></div>
    <div style="position:absolute;left:calc(6/12*100%);top:0;bottom:0;width:1px;background:#d6e0e6;"></div>
    <div style="position:absolute;left:calc(9/12*100%);top:0;bottom:0;width:1px;background:#d6e0e6;"></div>
    <div style="position:absolute;left:0;top:0;height:100%;width:calc(10/12*100%);display:flex;gap:2px;">
      <div style="flex:8;background:#0f4c5c;border-radius:4px 0 0 4px;display:flex;align-items:center;padding:0 10px;">
        <span style="font-size:0.75rem;color:rgba(255,255,255,0.88);white-space:nowrap;">8 published</span>
      </div>
      <div style="flex:2;background:#7bbfcb;border-radius:0 4px 4px 0;"></div>
    </div>
  </div>
  <div style="font-size:0.9rem;font-weight:700;color:var(--deck-ink);">10</div>

  <!-- INT'L CONFERENCES -->
  <div style="text-align:right; font-size:0.88rem; font-weight:600; color:var(--deck-ink); white-space:nowrap;">Int'l Conferences</div>
  <div style="position:relative; height:42px;">
    <div style="position:absolute;left:calc(3/12*100%);top:0;bottom:0;width:1px;background:rgba(255,255,255,0.3);"></div>
    <div style="position:absolute;left:calc(6/12*100%);top:0;bottom:0;width:1px;background:rgba(255,255,255,0.3);"></div>
    <div style="position:absolute;left:calc(9/12*100%);top:0;bottom:0;width:1px;background:rgba(255,255,255,0.3);"></div>
    <div style="position:absolute;left:0;top:0;height:100%;width:100%;background:#d97706;border-radius:4px;display:flex;align-items:center;justify-content:center;">
      <span style="font-size:0.78rem;color:rgba(255,255,255,0.92);white-space:nowrap;">12 international conferences</span>
    </div>
  </div>
  <div style="font-size:0.9rem;font-weight:700;color:var(--deck-ink);">12</div>

  <!-- WORKSHOPS -->
  <div style="text-align:right; font-size:0.88rem; font-weight:600; color:var(--deck-ink); white-space:nowrap;">Workshops</div>
  <div style="position:relative; height:42px;">
    <div style="position:absolute;left:calc(3/12*100%);top:0;bottom:0;width:1px;background:#d6e0e6;"></div>
    <div style="position:absolute;left:calc(6/12*100%);top:0;bottom:0;width:1px;background:#d6e0e6;"></div>
    <div style="position:absolute;left:calc(9/12*100%);top:0;bottom:0;width:1px;background:#d6e0e6;"></div>
    <div style="position:absolute;left:0;top:0;height:100%;width:calc(2/12*100%);background:#2f6b5b;border-radius:4px;"></div>
  </div>
  <div style="font-size:0.9rem;font-weight:700;color:var(--deck-ink);">2</div>

  <!-- DOCTORAL SYMPOSIUM -->
  <div style="text-align:right; font-size:0.88rem; font-weight:600; color:var(--deck-ink); white-space:nowrap;">Doctoral Symposium</div>
  <div style="position:relative; height:42px;">
    <div style="position:absolute;left:calc(3/12*100%);top:0;bottom:0;width:1px;background:#d6e0e6;"></div>
    <div style="position:absolute;left:calc(6/12*100%);top:0;bottom:0;width:1px;background:#d6e0e6;"></div>
    <div style="position:absolute;left:calc(9/12*100%);top:0;bottom:0;width:1px;background:#d6e0e6;"></div>
    <div style="position:absolute;left:0;top:0;height:100%;width:calc(2/12*100%);background:#8899a6;border-radius:4px;"></div>
  </div>
  <div style="font-size:0.9rem;font-weight:700;color:var(--deck-ink);">2</div>

  <!-- X-AXIS TICK LABELS -->
  <div></div>
  <div style="position:relative;height:16px;font-size:0.72rem;color:var(--deck-muted);">
    <span style="position:absolute;left:calc(3/12*100%);transform:translateX(-50%);">3</span>
    <span style="position:absolute;left:calc(6/12*100%);transform:translateX(-50%);">6</span>
    <span style="position:absolute;left:calc(9/12*100%);transform:translateX(-50%);">9</span>
    <span style="position:absolute;left:100%;transform:translateX(-50%);">12</span>
  </div>
  <div></div>

</div>

<div style="display:flex;gap:1.3rem;margin-top:0.5rem;padding-left:183px;font-size:0.75rem;color:var(--deck-muted);">
  <span style="display:flex;align-items:center;gap:5px;"><span style="display:inline-block;width:11px;height:11px;background:#0f4c5c;border-radius:2px;flex-shrink:0;"></span>Published</span>
  <span style="display:flex;align-items:center;gap:5px;"><span style="display:inline-block;width:11px;height:11px;background:#7bbfcb;border-radius:2px;flex-shrink:0;"></span>Under review</span>
</div>

</div>


---
layout: default
class: stage-slide
transition: slide-left
---

<div class="slide-shell">

# Publications

<div style="display:grid;grid-template-columns:1fr 1fr;gap:6px 20px;margin-top:8px;">
<div>
<div style="font-size:10px;font-weight:700;letter-spacing:0.08em;text-transform:uppercase;color:var(--deck-muted);border-bottom:1px solid var(--deck-line);padding-bottom:3px;margin-bottom:6px;">Journals</div>
<div style="margin-bottom:5px;display:flex;gap:5px;align-items:baseline;"><span style="font-size:10px;font-weight:700;color:var(--deck-muted);min-width:13px;">1.</span><div><div style="font-size:11px;font-weight:600;color:var(--deck-ink);line-height:1.25;">Decentralized proximity-aware clustering for collective self-federated learning</div><div style="font-size:10px;color:var(--deck-muted);">Domini, Farabegoli, Aguzzi, Viroli, Esterle &mdash; Internet of Things, Elsevier <span style="background:#0f4c5c;color:#fff;border-radius:3px;padding:0 3px;font-size:9px;font-weight:700;">Q1</span></div></div></div>
<div style="margin-bottom:5px;display:flex;gap:5px;align-items:baseline;"><span style="font-size:10px;font-weight:700;color:var(--deck-muted);min-width:13px;">2.</span><div><div style="font-size:11px;font-weight:600;color:var(--deck-ink);line-height:1.25;">Heterogeneous GNN for collective-task offloading in cloud-edge via deep Q-learning</div><div style="font-size:10px;color:var(--deck-muted);">Farabegoli, Domini, Aguzzi, Viroli &mdash; Future Gen. Computer Systems <span style="background:#0f4c5c;color:#fff;border-radius:3px;padding:0 3px;font-size:9px;font-weight:700;">Q1</span></div></div></div>
<div style="margin-bottom:5px;display:flex;gap:5px;align-items:baseline;"><span style="font-size:10px;font-weight:700;color:var(--deck-muted);min-width:13px;">3.</span><div><div style="font-size:11px;font-weight:600;color:var(--deck-ink);line-height:1.25;">Digital Twin Aggregates for adaptive MLOps retraining policies in healthcare</div><div style="font-size:10px;color:var(--deck-muted);">Domini, Micelli, Burattini, Montagna &mdash; Future Gen. Computer Systems <span style="background:#0f4c5c;color:#fff;border-radius:3px;padding:0 3px;font-size:9px;font-weight:700;">Q1</span></div></div></div>
<div style="margin-bottom:5px;display:flex;gap:5px;align-items:baseline;"><span style="font-size:10px;font-weight:700;color:var(--deck-muted);min-width:13px;">4.</span><div><div style="font-size:11px;font-weight:600;color:var(--deck-ink);line-height:1.25;">FBFL: A field-based coordination approach for data heterogeneity in federated learning</div><div style="font-size:10px;color:var(--deck-muted);">Domini, Aguzzi, Esterle, Viroli &mdash; Logical Methods in Computer Science <span style="background:#2f6b5b;color:#fff;border-radius:3px;padding:0 3px;font-size:9px;font-weight:700;">Q3</span></div></div></div>
<div style="margin-bottom:5px;display:flex;gap:5px;align-items:baseline;"><span style="font-size:10px;font-weight:700;color:var(--deck-muted);min-width:13px;">5.</span><div><div style="font-size:11px;font-weight:600;color:var(--deck-ink);line-height:1.25;">Profed: a benchmark for proximity-based non-iid federated learning</div><div style="font-size:10px;color:var(--deck-muted);">Domini, Ingemann, Aguzzi, Esterle, Viroli &mdash; J. of Open Research Software <span style="background:#2f6b5b;color:#fff;border-radius:3px;padding:0 3px;font-size:9px;font-weight:700;">Q3</span></div></div></div>
<div style="margin-bottom:5px;display:flex;gap:5px;align-items:baseline;"><span style="font-size:10px;font-weight:700;color:var(--deck-muted);min-width:13px;">6.</span><div><div style="font-size:11px;font-weight:600;color:var(--deck-ink);line-height:1.25;">Project Emerge: A Demonstrator for Self-organizing Robot Teams</div><div style="font-size:10px;color:var(--deck-muted);">Farabegoli, Aguzzi, Baiardi, Cortecchia, Domini, Pianini, Viroli &mdash; Science of Computer Programming <span style="background:#2f6b5b;color:#fff;border-radius:3px;padding:0 3px;font-size:9px;font-weight:700;">Q3</span></div></div></div>
<div style="margin-bottom:5px;display:flex;gap:5px;align-items:baseline;opacity:0.65;"><span style="font-size:10px;font-weight:700;color:var(--deck-muted);min-width:13px;">7.</span><div><div style="font-size:11px;font-weight:600;color:var(--deck-ink);line-height:1.25;">SParSeFuL: Self-Organizing Sparse Federated Learning over Spatially Non-IID Data <span style="font-weight:400;font-style:italic;font-size:10px;">&middot; under review</span></div><div style="font-size:10px;color:var(--deck-muted);">Domini et al. &mdash; Pervasive and Mobile Computing, Elsevier <span style="background:#0f4c5c;color:#fff;border-radius:3px;padding:0 3px;font-size:9px;font-weight:700;">Q1</span></div></div></div>
<div style="margin-bottom:0;display:flex;gap:5px;align-items:baseline;opacity:0.65;"><span style="font-size:10px;font-weight:700;color:var(--deck-muted);min-width:13px;">8.</span><div><div style="font-size:11px;font-weight:600;color:var(--deck-ink);line-height:1.25;">Digital Twin Aggregates for Hierarchical and Adaptive Collaborative Learning in Smart-Healthcare <span style="font-weight:400;font-style:italic;font-size:10px;">&middot; under review</span></div><div style="font-size:10px;color:var(--deck-muted);">Domini, Micelli, Burattini, Montagna &mdash; IEEE J. Biomed. and Health Informatics <span style="background:#0f4c5c;color:#fff;border-radius:3px;padding:0 3px;font-size:9px;font-weight:700;">Q1</span></div></div></div>
</div>
<div>
<div style="font-size:10px;font-weight:700;letter-spacing:0.08em;text-transform:uppercase;color:var(--deck-muted);border-bottom:1px solid var(--deck-line);padding-bottom:3px;margin-bottom:6px;">Conferences &amp; Workshops</div>
<div style="margin-bottom:5px;display:flex;gap:5px;align-items:baseline;"><span style="font-size:10px;font-weight:700;color:var(--deck-muted);min-width:13px;">1.</span><div><div style="font-size:11px;font-weight:600;color:var(--deck-ink);line-height:1.25;">Phyelds: A Pythonic Framework for Aggregate Computing</div><div style="font-size:10px;color:var(--deck-muted);">Aguzzi, Domini, Farabegoli, Viroli &mdash; Coordination 2026</div></div></div>
<div style="margin-bottom:5px;display:flex;gap:5px;align-items:baseline;"><span style="font-size:10px;font-weight:700;color:var(--deck-muted);min-width:13px;">2.</span><div><div style="font-size:11px;font-weight:600;color:var(--deck-ink);line-height:1.25;">HarmoniKt: a Unifying Middleware for Heterogeneous Robot Fleets</div><div style="font-size:10px;color:var(--deck-muted);">Andruccioli, Cortecchia, Domini et al. &mdash; IEEE CCNC 2026</div></div></div>
<div style="margin-bottom:5px;display:flex;gap:5px;align-items:baseline;"><span style="font-size:10px;font-weight:700;color:var(--deck-muted);min-width:13px;">3.</span><div><div style="font-size:11px;font-weight:600;color:var(--deck-ink);line-height:1.25;">C2FL: Clustered Continual Federated Learning under Spatial and Temporal Drift</div><div style="font-size:10px;color:var(--deck-muted);">Domini, Aguzzi, Pellegrini, Viroli, Esterle &mdash; ACSOS 2026</div></div></div>
<div style="margin-bottom:5px;display:flex;gap:5px;align-items:baseline;"><span style="font-size:10px;font-weight:700;color:var(--deck-muted);min-width:13px;">4.</span><div><div style="font-size:11px;font-weight:600;color:var(--deck-ink);line-height:1.25;">Discovering Collaboration from Novelty: RND for Clustered Federated Learning</div><div style="font-size:10px;color:var(--deck-muted);">Domini, Aguzzi, Dusparic, Pianini, Viroli &mdash; AI4AS 2026</div></div></div>
<div style="margin-bottom:5px;display:flex;gap:5px;align-items:baseline;"><span style="font-size:10px;font-weight:700;color:var(--deck-muted);min-width:13px;">5.</span><div><div style="font-size:11px;font-weight:600;color:var(--deck-ink);line-height:1.25;">Flexible Distributed Particle Filtering for the IoT via Aggregate Computing</div><div style="font-size:10px;color:var(--deck-muted);">Cortecchia, Domini, Ciatto, Casadei, Pianini, Viroli &mdash; DCOSS-IoT 2026</div></div></div>
<div style="margin-bottom:5px;display:flex;gap:5px;align-items:baseline;"><span style="font-size:10px;font-weight:700;color:var(--deck-muted);min-width:13px;">6.</span><div><div style="font-size:11px;font-weight:600;color:var(--deck-ink);line-height:1.25;">Multi-Target Tracking via Field-Based Distributed Particle Filtering</div><div style="font-size:10px;color:var(--deck-muted);">Cortecchia, Domini, Ciatto, Casadei, Viroli &mdash; ACSOS 2026</div></div></div>
<div style="margin-bottom:0;display:flex;gap:5px;align-items:baseline;"><span style="font-size:10px;font-weight:700;color:var(--deck-muted);min-width:13px;">7.</span><div><div style="font-size:11px;font-weight:600;color:var(--deck-ink);line-height:1.25;">Self-Organizing Federated Learning in Collective Adaptive Systems</div><div style="font-size:10px;color:var(--deck-muted);">Domini &mdash; ACSOS 2026, Doctoral Symposium</div></div></div>
</div>
</div>
</div>
---
layout: default
class: stage-slide
transition: slide-left
---

<div class="slide-shell">

# Conferences Attended

<div style="display:flex;flex-direction:column;gap:0;margin-top:1rem;">
<div style="display:flex;gap:5px;align-items:baseline;margin-bottom:8px;"><span style="font-size:11px;font-weight:700;color:var(--deck-muted);min-width:13px;">1.</span><div><div style="font-size:14px;font-weight:600;color:var(--deck-ink);line-height:1.3;">Coordination 2026</div><div style="font-size:12px;color:var(--deck-muted);">28th International Conference on Coordination Models and Languages &mdash; Urbino, Italy, June 8&ndash;12</div></div></div>
<div style="display:flex;gap:5px;align-items:baseline;margin-bottom:8px;"><span style="font-size:11px;font-weight:700;color:var(--deck-muted);min-width:13px;">2.</span><div><div style="font-size:14px;font-weight:600;color:var(--deck-ink);line-height:1.3;">ACSOS 2026</div><div style="font-size:12px;color:var(--deck-muted);">7th IEEE International Conference on Autonomic Computing and Self-Organizing Systems &mdash; Cesena, Italy, September 7&ndash;11</div></div></div>
<div style="display:flex;gap:5px;align-items:baseline;"><span style="font-size:11px;font-weight:700;color:var(--deck-muted);min-width:13px;">3.</span><div><div style="font-size:14px;font-weight:600;color:var(--deck-ink);line-height:1.3;">AI4AS 2026</div><div style="font-size:12px;color:var(--deck-muted);">4th International Workshop on Artificial Intelligence for Autonomous Computing Systems &mdash; Cesena, Italy, September 7&ndash;11</div></div></div>
</div>

</div>
---
layout: default
class: stage-slide
transition: slide-left
---

<div class="slide-shell">

# International Collaborations

<div style="margin-top:0.4rem;">
<img src="/figures/map.png" style="width:100%;max-height:430px;object-fit:contain;" />
</div>

</div>


---
layout: default
class: stage-slide
transition: slide-left
---

<div class="slide-shell">

# Scientific Service

<div style="display:flex;flex-direction:column;gap:1.1rem;margin-top:1.2rem;">

  <div style="display:flex;align-items:flex-start;gap:1.2rem;padding:1rem 1.2rem;border-radius:8px;border-left:3px solid var(--deck-teal);background:var(--deck-teal-soft);">
    <div style="flex-shrink:0;width:44px;height:44px;border-radius:8px;background:var(--deck-teal);display:flex;align-items:center;justify-content:center;">
      <svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="white" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="8" r="4"/><path d="M4 20c0-4 3.6-7 8-7s8 3 8 7"/><path d="M17 13l2 2 4-4"/></svg>
    </div>
    <div>
      <div style="font-weight:700;font-size:1rem;color:var(--deck-teal);">Social Experience Chair</div>
      <div style="font-size:0.88rem;color:var(--deck-muted);margin-top:0.2rem;">IEEE ACSOS 2026 — International Conference on Autonomic Computing and Self-Organizing Systems</div>
    </div>
  </div>

  <div style="display:flex;align-items:flex-start;gap:1.2rem;padding:1rem 1.2rem;border-radius:8px;border-left:3px solid var(--deck-orange);background:var(--deck-orange-soft);">
    <div style="flex-shrink:0;width:44px;height:44px;border-radius:8px;background:var(--deck-orange);display:flex;align-items:center;justify-content:center;">
      <svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="white" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="3" y="4" width="18" height="16" rx="2"/><path d="M16 2v4M8 2v4M3 10h18"/><path d="M8 14h2m-2 4h8"/></svg>
    </div>
    <div>
      <div style="font-weight:700;font-size:1rem;color:var(--deck-orange);">Program Chair</div>
      <div style="font-size:0.88rem;color:var(--deck-muted);margin-top:0.2rem;">AI4AS 2026 — 4th International Workshop on Artificial Intelligence for Autonomous computing Systems (co-located with ACSOS)</div>
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
