<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>TRAFFIQ — AI Traffic Intelligence</title>

<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

<style>
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: Inter, Arial, sans-serif;
}

html {
    scroll-behavior: smooth;
}

body {
    background: #050b14;
    color: #e8f5ff;
    overflow-x: hidden;
}

body::before {
    content: "";
    position: fixed;
    inset: 0;
    background:
        radial-gradient(circle at 15% 20%, rgba(0,180,255,.12), transparent 30%),
        radial-gradient(circle at 85% 70%, rgba(0,255,200,.08), transparent 30%);
    pointer-events: none;
    z-index: -1;
}

/* NAVBAR */

nav {
    position: sticky;
    top: 0;
    z-index: 100;
    height: 72px;
    padding: 0 5%;
    display: flex;
    align-items: center;
    justify-content: space-between;
    background: rgba(4,10,18,.92);
    backdrop-filter: blur(18px);
    border-bottom: 1px solid rgba(0,200,255,.15);
}

.logo {
    display: flex;
    align-items: center;
    gap: 12px;
    font-weight: 800;
    font-size: 21px;
    letter-spacing: 1px;
}

.logo-icon {
    width: 38px;
    height: 38px;
    border-radius: 10px;
    display: grid;
    place-items: center;
    background: linear-gradient(135deg,#00d9ff,#0066ff);
    color: white;
    box-shadow: 0 0 25px rgba(0,190,255,.45);
}

.nav-links {
    display: flex;
    gap: 20px;
}

.nav-links a {
    color: #91a8bc;
    text-decoration: none;
    font-size: 13px;
    transition: .3s;
}

.nav-links a:hover {
    color: #00d9ff;
}

.status {
    display: flex;
    align-items: center;
    gap: 8px;
    color: #55f5a6;
    font-size: 11px;
    font-weight: 700;
}

.status-dot {
    width: 8px;
    height: 8px;
    border-radius: 50%;
    background: #39f58c;
    box-shadow: 0 0 12px #39f58c;
}

/* HERO */

.hero {
    padding: 65px 5% 35px;
    max-width: 1500px;
    margin: auto;
}

.hero h1 {
    font-size: clamp(38px,6vw,72px);
    line-height: 1;
    margin-bottom: 18px;
    background: linear-gradient(90deg,#fff,#00d9ff,#7d8cff);
    -webkit-background-clip: text;
    color: transparent;
}

.hero p {
    max-width: 760px;
    color: #8da5b8;
    line-height: 1.7;
    font-size: 16px;
}

.hero-actions {
    display: flex;
    gap: 12px;
    margin-top: 28px;
}

button {
    border: 0;
    cursor: pointer;
    color: white;
    font-weight: 700;
    border-radius: 9px;
    padding: 12px 18px;
    transition: .25s;
}

.primary {
    background: linear-gradient(135deg,#00c8ff,#006aff);
    box-shadow: 0 0 20px rgba(0,150,255,.25);
}

.primary:hover {
    transform: translateY(-2px);
    box-shadow: 0 0 30px rgba(0,180,255,.45);
}

.secondary {
    background: rgba(255,255,255,.05);
    border: 1px solid rgba(255,255,255,.12);
}

.secondary:hover {
    background: rgba(255,255,255,.1);
}

/* CONTAINER */

.container {
    max-width: 1500px;
    margin: auto;
    padding: 0 5% 70px;
}

.section {
    margin-top: 65px;
    scroll-margin-top: 90px;
}

.section-title {
    margin-bottom: 22px;
}

.section-title h2 {
    font-size: 28px;
}

.section-title p {
    color: #70899d;
    margin-top: 6px;
}

/* CARDS */

.grid {
    display: grid;
    gap: 16px;
}

.stats-grid {
    grid-template-columns: repeat(5,1fr);
}

.card {
    background: linear-gradient(
        145deg,
        rgba(18,34,52,.78),
        rgba(7,17,29,.72)
    );
    border: 1px solid rgba(115,190,255,.12);
    border-radius: 16px;
    padding: 21px;
    backdrop-filter: blur(15px);
    box-shadow: inset 0 1px rgba(255,255,255,.03),
                0 15px 40px rgba(0,0,0,.2);
}

.stat-label {
    color: #7e95a8;
    font-size: 12px;
}

.stat-value {
    font-size: 31px;
    font-weight: 800;
    margin: 9px 0;
}

.stat-change {
    color: #42e899;
    font-size: 11px;
}

/* TRAFFIC STATUS */

.status-bar {
    display: grid;
    grid-template-columns: repeat(4,1fr);
    gap: 10px;
}

.traffic-level {
    padding: 15px;
    border-radius: 12px;
    text-align: center;
    font-weight: 800;
    font-size: 12px;
    border: 1px solid rgba(255,255,255,.08);
}

.low {
    background: rgba(40,220,130,.1);
    color: #42ee94;
}

.moderate {
    background: rgba(255,195,55,.1);
    color: #ffc83d;
}

.high {
    background: rgba(255,120,40,.1);
    color: #ff8d45;
}

.critical {
    background: rgba(255,55,75,.1);
    color: #ff5667;
}

/* JUNCTIONS */

.junction-grid {
    grid-template-columns: repeat(4,1fr);
}

.junction-card {
    position: relative;
    overflow: hidden;
}

.junction-top {
    display: flex;
    justify-content: space-between;
}

.junction-name {
    font-size: 17px;
    font-weight: 800;
}

.density {
    margin: 20px 0 10px;
}

.progress {
    height: 7px;
    background: #162433;
    border-radius: 10px;
    overflow: hidden;
}

.progress span {
    display: block;
    height: 100%;
    border-radius: inherit;
    transition: width .8s;
}

.green { background: #27e889; }
.yellow { background: #ffc83d; }
.red { background: #ff5365; }

.junction-info {
    margin-top: 18px;
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 12px;
}

.info small {
    color: #6d8498;
    display: block;
    font-size: 10px;
}

.info strong {
    display: block;
    margin-top: 4px;
}

/* MAP */

.map {
    min-height: 420px;
    position: relative;
    overflow: hidden;
    background:
        linear-gradient(rgba(0,180,255,.035) 1px,transparent 1px),
        linear-gradient(90deg,rgba(0,180,255,.035) 1px,transparent 1px);
    background-size: 40px 40px;
}

.road {
    position: absolute;
    background: #172533;
}

.road.h {
    left: 0;
    right: 0;
    height: 55px;
    top: 50%;
    transform: translateY(-50%);
}

.road.v {
    top: 0;
    bottom: 0;
    width: 55px;
    left: 50%;
    transform: translateX(-50%);
}

.junction-point {
    position: absolute;
    width: 20px;
    height: 20px;
    border-radius: 50%;
    background: #00d9ff;
    box-shadow: 0 0 25px #00d9ff;
}

.j1 { left: 24%; top: 30%; }
.j2 { right: 23%; top: 30%; }
.j3 { left: 24%; bottom: 25%; }
.j4 { right: 23%; bottom: 25%; }

.map-label {
    position: absolute;
    padding: 7px 10px;
    background: rgba(5,15,25,.9);
    border: 1px solid rgba(0,200,255,.2);
    border-radius: 7px;
    font-size: 11px;
}

/* TWO COLUMN */

.two-col {
    display: grid;
    grid-template-columns: 1.4fr 1fr;
    gap: 18px;
}

.chart-container {
    height: 300px;
}

/* AI PREDICTION */

.prediction-grid {
    display: grid;
    grid-template-columns: repeat(4,1fr);
    gap: 12px;
    margin-bottom: 20px;
}

.metric {
    padding: 15px;
    background: rgba(0,0,0,.2);
    border-radius: 11px;
}

.metric small {
    color: #70889b;
}

.metric h3 {
    margin-top: 8px;
    font-size: 25px;
}

.recommendation {
    padding: 18px;
    border-left: 3px solid #00d9ff;
    background: rgba(0,200,255,.05);
    line-height: 1.6;
    color: #b7cbd9;
}

/* SIGNAL */

.signal-grid {
    display: grid;
    grid-template-columns: repeat(4,1fr);
    gap: 12px;
}

.signal {
    text-align: center;
    padding: 20px;
}

.signal-light {
    width: 70px;
    height: 70px;
    margin: auto auto 14px;
    border-radius: 50%;
    background: #0b151f;
    border: 3px solid #20394d;
    display: grid;
    place-items: center;
    color: #31ed93;
    font-weight: 900;
    box-shadow: 0 0 20px rgba(50,230,150,.15);
}

.timer {
    font-size: 27px;
    font-weight: 900;
    color: #00d9ff;
}

.signal-name {
    color: #849caf;
    margin-bottom: 7px;
}

/* EMERGENCY */

.emergency {
    border: 1px solid rgba(255,70,80,.35);
    background:
        linear-gradient(135deg,rgba(255,50,70,.08),rgba(10,20,32,.8));
}

.emergency-header {
    display: flex;
    align-items: center;
    gap: 15px;
}

.ambulance {
    font-size: 42px;
}

.route {
    display: flex;
    align-items: center;
    gap: 0;
    margin: 30px 0;
}

.route-node {
    width: 45px;
    height: 45px;
    border-radius: 50%;
    display: grid;
    place-items: center;
    background: #162535;
    border: 2px solid #536c7e;
    font-size: 11px;
    z-index: 2;
}

.route-node.active {
    background: #00d995;
    color: #02140d;
    border-color: #00ffad;
    box-shadow: 0 0 20px #00d995;
}

.route-line {
    flex: 1;
    height: 3px;
    background: #334b5d;
}

.route-line.active {
    background: #00e69a;
    box-shadow: 0 0 12px #00e69a;
}

.success {
    color: #4affac;
    font-weight: 800;
    margin-top: 14px;
}

/* ALERTS */

.alert {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 15px;
    margin-bottom: 10px;
    background: rgba(0,0,0,.2);
    border-radius: 10px;
    border-left: 3px solid #ff5365;
}

.alert.warning {
    border-left-color: #ffc83d;
}

.alert.resolved {
    opacity: .45;
}

.alert-info strong {
    display: block;
}

.alert-info small {
    color: #748b9d;
}

/* FEATURES */

.feature-grid {
    grid-template-columns: repeat(4,1fr);
}

.feature {
    min-height: 125px;
}

.feature-icon {
    font-size: 25px;
    margin-bottom: 12px;
}

.feature h3 {
    font-size: 14px;
    margin-bottom: 7px;
}

.feature p {
    color: #71899c;
    font-size: 12px;
    line-height: 1.5;
}

/* DEMO */

.demo-banner {
    display: none;
    padding: 12px 18px;
    background: rgba(0,210,255,.08);
    border: 1px solid rgba(0,210,255,.25);
    border-radius: 10px;
    margin-bottom: 18px;
    color: #63ddff;
}

.demo-banner.active {
    display: block;
}

/* TOAST */

#toast {
    position: fixed;
    right: 25px;
    bottom: 25px;
    background: #102334;
    border: 1px solid #1c789d;
    padding: 15px 20px;
    border-radius: 10px;
    transform: translateY(120px);
    transition: .4s;
    z-index: 999;
    box-shadow: 0 10px 35px rgba(0,0,0,.4);
}

#toast.show {
    transform: translateY(0);
}

/* FOOTER */

footer {
    text-align: center;
    padding: 40px 20px;
    border-top: 1px solid rgba(255,255,255,.08);
    color: #6f8799;
}

footer strong {
    color: #00d9ff;
}

/* RESPONSIVE */

@media(max-width:1100px) {
    .stats-grid,
    .junction-grid,
    .feature-grid {
        grid-template-columns: repeat(2,1fr);
    }

    .nav-links {
        display: none;
    }
}

@media(max-width:750px) {
    .stats-grid,
    .junction-grid,
    .feature-grid,
    .signal-grid,
    .prediction-grid,
    .status-bar {
        grid-template-columns: 1fr;
    }

    .two-col {
        grid-template-columns: 1fr;
    }

    .hero {
        padding-top: 40px;
    }

    .hero-actions {
        flex-direction: column;
    }

    .route-node {
        width: 35px;
        height: 35px;
        font-size: 9px;
    }
}
</style>
</head>

<body>

<!-- NAVIGATION -->

<nav>
    <div class="logo">
        <div class="logo-icon">T</div>
        TRAFFIQ
    </div>

    <div class="nav-links">
        <a href="#dashboard">Dashboard</a>
        <a href="#traffic">Live Traffic</a>
        <a href="#prediction">AI Prediction</a>
        <a href="#signals">Signal Control</a>
        <a href="#emergency">Emergency</a>
        <a href="#incidents">Incidents</a>
        <a href="#analytics">Analytics</a>
    </div>

    <div class="status">
        <span class="status-dot"></span>
        AI SYSTEM ONLINE
    </div>
</nav>


<!-- HERO -->

<header class="hero" id="dashboard">
    <h1>AI-Powered<br>Traffic Intelligence</h1>

    <p>
        TRAFFIQ transforms real-time traffic data into intelligent decisions.
        Predict congestion, dynamically optimize signal timings, detect incidents
        and create emergency green corridors before traffic becomes a problem.
    </p>

    <div class="hero-actions">
        <button class="primary" onclick="toggleDemo()">
            ▶ DEMO MODE
        </button>

        <button class="secondary" onclick="scrollToSection('traffic')">
            View Live Traffic
        </button>
    </div>
</header>


<main class="container">

<div id="demoBanner" class="demo-banner">
    ⚡ DEMO MODE ACTIVE — Simulating live traffic conditions and AI decisions
</div>


<!-- SYSTEM STATS -->

<section class="section">
    <div class="section-title">
        <h2>Smart City Command Center</h2>
        <p>Real-time simulated intelligence from connected urban infrastructure</p>
    </div>

    <div class="grid stats-grid">

        <div class="card">
            <span class="stat-label">Vehicles Detected</span>
            <div class="stat-value" id="vehicles">1,284</div>
            <span class="stat-change">↑ 8.4% live flow</span>
        </div>

        <div class="card">
            <span class="stat-label">Average Speed</span>
            <div class="stat-value" id="speed">32</div>
            <span class="stat-change">km/h</span>
        </div>

        <div class="card">
            <span class="stat-label">Active Signals</span>
            <div class="stat-value">12</div>
            <span class="stat-change">AI controlled</span>
        </div>

        <div class="card">
            <span class="stat-label">Congestion Alerts</span>
            <div class="stat-value" id="alertsCount">3</div>
            <span class="stat-change">Requires attention</span>
        </div>

        <div class="card">
            <span class="stat-label">Emergency Vehicles</span>
            <div class="stat-value">1</div>
            <span class="stat-change">Priority enabled</span>
        </div>

    </div>
</section>


<!-- TRAFFIC LEVEL -->

<section class="section">
    <div class="section-title">
        <h2>Live Traffic Status</h2>
        <p>AI classification of current network conditions</p>
    </div>

    <div class="status-bar">
        <div class="traffic-level low">LOW</div>
        <div class="traffic-level moderate">MODERATE</div>
        <div class="traffic-level high">HIGH</div>
        <div class="traffic-level critical">CRITICAL</div>
    </div>
</section>


<!-- LIVE MAP -->

<section class="section" id="traffic">
    <div class="section-title">
        <h2>Live Traffic Monitoring</h2>
        <p>Four major junctions monitored by the TRAFFIQ intelligence engine</p>
    </div>

    <div class="two-col">

        <div class="card map">

            <div class="road h"></div>
            <div class="road v"></div>

            <div class="junction-point j1"></div>
            <div class="junction-point j2"></div>
            <div class="junction-point j3"></div>
            <div class="junction-point j4"></div>

            <div class="map-label" style="left:15%;top:20%">
                Junction A
            </div>

            <div class="map-label" style="right:15%;top:20%">
                Junction B
            </div>

            <div class="map-label" style="left:15%;bottom:15%">
                Junction C
            </div>

            <div class="map-label" style="right:15%;bottom:15%">
                Junction D
            </div>

        </div>

        <div class="grid junction-grid">

            <div class="card junction-card">
                <div class="junction-top">
                    <span class="junction-name">Junction A</span>
                    <span class="low">LOW</span>
                </div>

                <div class="density">
                    <span>Traffic Density <b id="densityA">42%</b></span>
                    <div class="progress">
                        <span class="green" id="barA" style="width:42%"></span>
                    </div>
                </div>

                <div class="junction-info">
                    <div class="info">
                        <small>Vehicles</small>
                        <strong id="vehiclesA">316</strong>
                    </div>

                    <div class="info">
                        <small>Signal</small>
                        <strong>GREEN</strong>
                    </div>

                    <div class="info">
                        <small>Countdown</small>
                        <strong>28 sec</strong>
                    </div>

                    <div class="info">
                        <small>Status</small>
                        <strong>Normal</strong>
                    </div>
                </div>
            </div>


            <div class="card junction-card">
                <div class="junction-top">
                    <span class="junction-name">Junction B</span>
                    <span class="high">HIGH</span>
                </div>

                <div class="density">
                    <span>Traffic Density <b id="densityB">78%</b></span>
                    <div class="progress">
                        <span class="red" id="barB" style="width:78%"></span>
                    </div>
                </div>

                <div class="junction-info">
                    <div class="info">
                        <small>Vehicles</small>
                        <strong id="vehiclesB">582</strong>
                    </div>

                    <div class="info">
                        <small>Signal</small>
                        <strong>RED</strong>
                    </div>

                    <div class="info">
                        <small>Countdown</small>
                        <strong>14 sec</strong>
                    </div>

                    <div class="info">
                        <small>Status</small>
                        <strong>Congested</strong>
                    </div>
                </div>
            </div>


            <div class="card junction-card">
                <div class="junction-top">
                    <span class="junction-name">Junction C</span>
                    <span class="moderate">MODERATE</span>
                </div>

                <div class="density">
                    <span>Traffic Density <b id="densityC">61%</b></span>
                    <div class="progress">
                        <span class="yellow" id="barC" style="width:61%"></span>
                    </div>
                </div>

                <div class="junction-info">
                    <div class="info">
                        <small>Vehicles</small>
                        <strong id="vehiclesC">421</strong>
                    </div>

                    <div class="info">
                        <small>Signal</small>
                        <strong>GREEN</strong>
                    </div>

                    <div class="info">
                        <small>Countdown</small>
                        <strong>35 sec</strong>
                    </div>

                    <div class="info">
                        <small>Status</small>
                        <strong>Monitoring</strong>
                    </div>
                </div>
            </div>


            <div class="card junction-card">
                <div class="junction-top">
                    <span class="junction-name">Junction D</span>
                    <span class="low">LOW</span>
                </div>

                <div class="density">
                    <span>Traffic Density <b id="densityD">29%</b></span>
                    <div class="progress">
                        <span class="green" id="barD" style="width:29%"></span>
                    </div>
                </div>

                <div class="junction-info">
                    <div class="info">
                        <small>Vehicles</small>
                        <strong id="vehiclesD">187</strong>
                    </div>

                    <div class="info">
                        <small>Signal</small>
                        <strong>GREEN</strong>
                    </div>

                    <div class="info">
                        <small>Countdown</small>
                        <strong>42 sec</strong>
                    </div>

                    <div class="info">
                        <small>Status</small>
                        <strong>Free Flow</st
