<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Pro Tipster - Análisis Avanzado</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800;900&display=swap" rel="stylesheet" />
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        body {
            font-family: 'Inter', sans-serif;
            background: #0a0e1a;
            color: #e8edf5;
            min-height: 100vh;
            padding: 16px;
        }
        ::-webkit-scrollbar {
            width: 8px;
            height: 8px;
        }
        ::-webkit-scrollbar-track {
            background: #141b2b;
            border-radius: 10px;
        }
        ::-webkit-scrollbar-thumb {
            background: linear-gradient(180deg, #f7c948, #f5a623);
            border-radius: 10px;
        }
        .app-container {
            max-width: 1440px;
            margin: 0 auto;
        }
        .header {
            background: linear-gradient(135deg, #111827, #1a2332);
            border-radius: 20px;
            padding: 24px 32px;
            margin-bottom: 24px;
            border: 1px solid rgba(255, 255, 255, 0.06);
            box-shadow: 0 20px 40px rgba(0, 0, 0, 0.5);
            display: flex;
            flex-wrap: wrap;
            justify-content: space-between;
            align-items: center;
            gap: 16px;
        }
        .header-left {
            display: flex;
            align-items: center;
            gap: 16px;
        }
        .logo-icon {
            width: 48px;
            height: 48px;
            background: linear-gradient(135deg, #f7c948, #f5a623);
            border-radius: 14px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 24px;
            box-shadow: 0 8px 20px rgba(247, 201, 72, 0.25);
        }
        .header-title h1 {
            font-size: 24px;
            font-weight: 800;
            background: linear-gradient(135deg, #f7c948, #f5a623);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
            letter-spacing: -0.5px;
        }
        .header-title p {
            font-size: 12px;
            color: #8ba0b8;
            font-weight: 400;
            margin-top: 2px;
        }
        .header-right {
            display: flex;
            align-items: center;
            gap: 14px;
            flex-wrap: wrap;
        }
        .date-badge {
            background: #1e2d42;
            padding: 8px 20px;
            border-radius: 60px;
            font-size: 13px;
            font-weight: 500;
            color: #b6d0e8;
            border: 1px solid #2a4058;
            display: flex;
            align-items: center;
            gap: 8px;
        }
        .date-badge .dot {
            width: 8px;
            height: 8px;
            background: #4ade80;
            border-radius: 50%;
            display: inline-block;
            animation: pulse-dot 2s infinite;
        }
        .date-badge .dot.pending {
            background: #60a5fa;
        }
        .date-badge .dot.finished {
            background: #6b7280;
            animation: none;
        }
        @keyframes pulse-dot {
            0%,
            100% {
                opacity: 1;
                transform: scale(1);
            }
            50% {
                opacity: 0.5;
                transform: scale(0.8);
            }
        }
        .api-section {
            background: linear-gradient(135deg, #111827, #1a2332);
            border-radius: 14px;
            padding: 16px 24px;
            margin-bottom: 20px;
            border: 1px solid rgba(255, 255, 255, 0.05);
            display: flex;
            flex-wrap: wrap;
            align-items: center;
            gap: 14px;
        }
        .api-section label {
            font-weight: 600;
            font-size: 13px;
            color: #b6d0e8;
            display: flex;
            align-items: center;
            gap: 8px;
        }
        .api-input-group {
            flex: 1;
            min-width: 200px;
            display: flex;
            gap: 10px;
            flex-wrap: wrap;
        }
        .api-input-group input {
            flex: 1;
            min-width: 150px;
            padding: 10px 16px;
            border-radius: 10px;
            border: 1px solid #2a4058;
            background: #0d1624;
            color: #e8edf5;
            font-size: 13px;
            font-family: 'Inter', sans-serif;
            transition: all 0.3s;
            outline: none;
        }
        .api-input-group input:focus {
            border-color: #f7c948;
            box-shadow: 0 0 0 3px rgba(247, 201, 72, 0.12);
        }
        .api-input-group input::placeholder {
            color: #4a6078;
        }
        .btn {
            padding: 10px 24px;
            border: none;
            border-radius: 10px;
            font-weight: 700;
            font-size: 13px;
            cursor: pointer;
            transition: all 0.3s ease;
            font-family: 'Inter', sans-serif;
            display: inline-flex;
            align-items: center;
            gap: 8px;
            white-space: nowrap;
        }
        .btn-primary {
            background: linear-gradient(135deg, #f7c948, #f5a623);
            color: #0a0e1a;
            box-shadow: 0 6px 20px rgba(247, 201, 72, 0.2);
        }
        .btn-primary:hover {
            transform: translateY(-2px);
            box-shadow: 0 10px 28px rgba(247, 201, 72, 0.35);
        }
        .btn-primary:disabled {
            opacity: 0.5;
            cursor: not-allowed;
            transform: none;
        }
        .btn-secondary {
            background: #1e2d42;
            color: #b6d0e8;
            border: 1px solid #2a4058;
        }
        .btn-secondary:hover {
            background: #2a4058;
        }
        .btn-success {
            background: #22c55e;
            color: #0a0e1a;
        }
        .btn-success:hover {
            background: #16a34a;
        }
        .btn-refresh {
            background: linear-gradient(135deg, #3b82f6, #2563eb);
            color: white;
            box-shadow: 0 6px 20px rgba(59, 130, 246, 0.2);
        }
        .btn-refresh:hover {
            transform: translateY(-2px);
            box-shadow: 0 10px 28px rgba(59, 130, 246, 0.35);
        }
        .btn-refresh:disabled {
            opacity: 0.5;
            cursor: not-allowed;
            transform: none;
        }
        .status-bar {
            padding: 10px 18px;
            border-radius: 10px;
            margin-bottom: 20px;
            font-size: 13px;
            font-weight: 500;
            display: flex;
            align-items: center;
            gap: 10px;
            background: #111827;
            border-left: 4px solid #4ade80;
            border: 1px solid rgba(255, 255, 255, 0.05);
        }
        .status-bar.error {
            border-left-color: #ef4444;
            background: #1f1414;
        }
        .status-bar .spinner {
            width: 16px;
            height: 16px;
            border: 2px solid rgba(255, 255, 255, 0.1);
            border-top-color: #f7c948;
            border-radius: 50%;
            animation: spin 0.8s linear infinite;
        }
        @keyframes spin {
            to {
                transform: rotate(360deg);
            }
        }
        .tabs {
            display: flex;
            gap: 6px;
            margin-bottom: 20px;
            flex-wrap: wrap;
        }
        .tab-btn {
            padding: 10px 20px;
            border: none;
            border-radius: 10px;
            font-weight: 600;
            font-size: 13px;
            cursor: pointer;
            transition: all 0.3s ease;
            font-family: 'Inter', sans-serif;
            background: #111827;
            color: #8ba0b8;
            border: 1px solid transparent;
        }
        .tab-btn:hover {
            background: #1a2332;
            color: #e8edf5;
        }
        .tab-btn.active {
            background: linear-gradient(135deg, #f7c948, #f5a623);
            color: #0a0e1a;
            box-shadow: 0 4px 16px rgba(247, 201, 72, 0.2);
        }
        .tab-content {
            display: none;
        }
        .tab-content.active {
            display: block;
        }
        .matches-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(360px, 1fr));
            gap: 16px;
            margin-bottom: 24px;
        }
        .match-card {
            background: linear-gradient(145deg, #111827, #1a2332);
            border-radius: 14px;
            padding: 16px 18px;
            border: 1px solid rgba(255, 255, 255, 0.05);
            transition: all 0.3s ease;
            position: relative;
        }
        .match-card:hover {
            transform: translateY(-3px);
            border-color: rgba(247, 201, 72, 0.15);
            box-shadow: 0 10px 28px rgba(0, 0, 0, 0.4);
        }
        .match-card.top-match {
            border-color: #f7c948;
            background: linear-gradient(145deg, #1a2332, #1f2d42);
        }
        .match-card .badge-top {
            position: absolute;
            top: 10px;
            right: 12px;
            background: linear-gradient(135deg, #f7c948, #f5a623);
            color: #0a0e1a;
            font-size: 10px;
            font-weight: 700;
            padding: 2px 12px;
            border-radius: 20px;
            letter-spacing: 0.5px;
        }
        .match-card .match-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 8px;
            font-size: 11px;
            color: #6a8aa8;
        }
        .match-card .match-header .league {
            background: #0d1624;
            padding: 2px 12px;
            border-radius: 20px;
            font-weight: 500;
        }
        .match-card .match-header .time {
            font-weight: 500;
        }
        .match-card .teams {
            display: flex;
            justify-content: space-between;
            align-items: center;
            font-size: 15px;
            font-weight: 600;
            margin: 4px 0 10px;
            gap: 8px;
            flex-wrap: wrap;
        }
        .match-card .teams .team {
            display: flex;
            align-items: center;
            gap: 8px;
            flex: 1;
        }
        .match-card .teams .team.away {
            justify-content: flex-end;
        }
        .match-card .teams .team .flag {
            font-size: 18px;
        }
        .match-card .teams .vs {
            font-size: 11px;
            color: #4a6078;
            font-weight: 400;
            flex-shrink: 0;
        }
        .match-card .teams .team .result-badge {
            font-size: 11px;
            font-weight: 700;
            padding: 2px 10px;
            border-radius: 20px;
            margin-left: 6px;
        }
        .result-over {
            background: #86efac;
            color: #064e3b;
        }
        .result-under {
            background: #fca5a5;
            color: #7f1d1d;
        }
        .result-pending {
            background: #93c5fd;
            color: #1e3a5f;
        }
        .result-draw {
            background: #fcd34d;
            color: #78350f;
        }
        .result-win {
            background: #6ee7b7;
            color: #064e3b;
        }
        .result-loss {
            background: #fca5a5;
            color: #7f1d1d;
        }
        .tipster-stats {
            display: grid;
            grid-template-columns: 1fr 1fr 1fr;
            gap: 6px;
            margin: 10px 0;
            padding: 10px;
            background: #0d1624;
            border-radius: 10px;
        }
        .tipster-stats .stat {
            text-align: center;
        }
        .tipster-stats .stat .label {
            font-size: 9px;
            text-transform: uppercase;
            color: #4a6078;
            letter-spacing: 0.5px;
            font-weight: 600;
        }
        .tipster-stats .stat .value {
            font-size: 15px;
            font-weight: 700;
            color: #e8edf5;
            display: block;
            margin-top: 2px;
        }
        .tipster-stats .stat .value.gold {
            color: #f7c948;
        }
        .tipster-stats .stat .value.green {
            color: #4ade80;
        }
        .tipster-stats .stat .value.red {
            color: #f87171;
        }
        .match-card .prob-bar {
            display: flex;
            height: 4px;
            border-radius: 4px;
            overflow: hidden;
            margin-top: 8px;
            background: #0d1624;
        }
        .match-card .prob-bar .bar-local {
            background: linear-gradient(90deg, #f7c948, #f5a623);
            height: 100%;
            transition: width 0.6s ease;
        }
        .match-card .prob-bar .bar-draw {
            background: #4a6078;
            height: 100%;
            transition: width 0.6s ease;
        }
        .match-card .prob-bar .bar-away {
            background: #60a5fa;
            height: 100%;
            transition: width 0.6s ease;
        }
        .match-card .prob-labels {
            display: flex;
            justify-content: space-between;
            font-size: 10px;
            color: #6a8aa8;
            margin-top: 4px;
        }
        .match-card .prediction-line {
            margin-top: 10px;
            padding-top: 10px;
            border-top: 1px solid rgba(255, 255, 255, 0.05);
            font-size: 12px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            flex-wrap: wrap;
            gap: 6px;
        }
        .match-card .prediction-line .favorite {
            color: #f7c948;
            font-weight: 600;
        }
        .match-card .prediction-line .over-under {
            color: #4ade80;
            font-weight: 600;
        }
        .fija-section {
            background: linear-gradient(135deg, #1a2332, #1f2d42);
            border-radius: 20px;
            padding: 28px 32px;
            margin: 0 0 24px 0;
            border: 2px solid #f7c948;
            box-shadow: 0 0 40px rgba(247, 201, 72, 0.06);
            position: relative;
            overflow: hidden;
        }
        .fija-section::before {
            content: '';
            position: absolute;
            top: -50%;
            right: -20%;
            width: 300px;
            height: 300px;
            background: radial-gradient(circle, rgba(247, 201, 72, 0.05), transparent);
            border-radius: 50%;
        }
        .fija-header {
            display: flex;
            align-items: center;
            gap: 14px;
            margin-bottom: 12px;
            flex-wrap: wrap;
        }
        .fija-header .fija-icon {
            font-size: 28px;
        }
        .fija-header h2 {
            font-size: 20px;
            font-weight: 700;
            color: #f7c948;
        }
        .fija-header .fija-badge {
            background: #f7c948;
            color: #0a0e1a;
            padding: 2px 14px;
            border-radius: 20px;
            font-size: 11px;
            font-weight: 700;
            margin-left: auto;
        }
        .fija-match {
            display: flex;
            flex-wrap: wrap;
            align-items: center;
            gap: 16px;
            margin: 6px 0 12px;
        }
        .fija-match .team-name {
            font-size: 22px;
            font-weight: 700;
        }
        .fija-match .team-name.local {
            color: #f7c948;
        }
        .fija-match .vs-text {
            font-size: 14px;
            color: #4a6078;
            font-weight: 400;
        }
        .fija-match .prediction {
            background: #0d1624;
            padding: 4px 18px;
            border-radius: 40px;
            font-size: 13px;
            font-weight: 600;
            color: #4ade80;
        }
        .fija-stats-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(140px, 1fr));
            gap: 12px;
            margin: 14px 0;
            padding: 14px;
            background: #0d1624;
            border-radius: 12px;
        }
        .fija-stats-grid .fs-item {
            text-align: center;
        }
        .fija-stats-grid .fs-item .fs-label {
            font-size: 10px;
            text-transform: uppercase;
            color: #4a6078;
            font-weight: 600;
            letter-spacing: 0.5px;
        }
        .fija-stats-grid .fs-item .fs-value {
            font-size: 18px;
            font-weight: 700;
            color: #e8edf5;
            display: block;
            margin-top: 2px;
        }
        .fija-stats-grid .fs-item .fs-value.gold {
            color: #f7c948;
        }
        .fija-stats-grid .fs-item .fs-value.green {
            color: #4ade80;
        }
        .fija-stats-grid .fs-item .fs-value.red {
            color: #f87171;
        }
        .fija-args {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
            gap: 12px;
            margin-top: 14px;
        }
        .fija-args .arg-item {
            background: #0d1624;
            padding: 12px 16px;
            border-radius: 10px;
            border-left: 3px solid #f7c948;
        }
        .fija-args .arg-item .arg-num {
            font-size: 10px;
            color: #f7c948;
            font-weight: 700;
            text-transform: uppercase;
            letter-spacing: 0.5px;
        }
        .fija-args .arg-item .arg-text {
            font-size: 12px;
            color: #b6d0e8;
            margin-top: 4px;
            line-height: 1.5;
        }
        .empty-state {
            text-align: center;
            padding: 50px 20px;
            color: #4a6078;
        }
        .empty-state .empty-icon {
            font-size: 40px;
            margin-bottom: 12px;
        }
        .empty-state h3 {
            font-size: 18px;
            color: #8ba0b8;
            margin-bottom: 6px;
        }
        .empty-state p {
            font-size: 13px;
        }
        .count-badge {
            background: #f7c94820;
            color: #f7c948;
            padding: 2px 12px;
            border-radius: 20px;
            font-size: 12px;
            font-weight: 600;
            margin-left: 8px;
        }
        .data-source-badge {
            font-size: 10px;
            color: #4a6078;
            background: #0d1624;
            padding: 2px 10px;
            border-radius: 20px;
            margin-left: 8px;
        }
        .demo-data-banner {
            background: #1e2d42;
            border: 1px solid #f7c94840;
            border-radius: 10px;
            padding: 12px 18px;
            margin-bottom: 16px;
            color: #b6d0e8;
            font-size: 13px;
            display: flex;
            align-items: center;
            gap: 12px;
        }
        .demo-data-banner strong {
            color: #f7c948;
        }
        .header-actions {
            display: flex;
            gap: 10px;
            flex-wrap: wrap;
        }
        @media (max-width: 768px) {
            .header {
                padding: 16px 20px;
            }
            .header-title h1 {
                font-size: 18px;
            }
            .api-section {
                flex-direction: column;
                align-items: stretch;
            }
            .api-input-group {
                flex-direction: column;
            }
            .matches-grid {
                grid-template-columns: 1fr;
            }
            .fija-section {
                padding: 18px 16px;
            }
            .fija-match {
                flex-direction: column;
                align-items: flex-start;
            }
            .fija-match .team-name {
                font-size: 18px;
            }
            .fija-args {
                grid-template-columns: 1fr;
            }
            .fija-stats-grid {
                grid-template-columns: 1fr 1fr;
            }
            .tipster-stats {
                grid-template-columns: 1fr 1fr;
            }
            .header-right {
                width: 100%;
            }
            .date-badge {
                width: 100%;
                justify-content: center;
            }
            .header-actions {
                width: 100%;
            }
            .header-actions .btn {
                flex: 1;
                justify-content: center;
            }
        }
        @media (max-width: 480px) {
            .tipster-stats {
                grid-template-columns: 1fr 1fr;
            }
            .fija-stats-grid {
                grid-template-columns: 1fr 1fr;
            }
            .match-card .teams {
                font-size: 13px;
                flex-wrap: wrap;
                justify-content: center;
            }
            .match-card .teams .team {
                flex: 0 0 100%;
                justify-content: center;
            }
            .match-card .teams .team.away {
                justify-content: center;
            }
        }
    </style>
</head>
<body>

    <div class="app-container">
        <header class="header">
            <div class="header-left">
                <div class="logo-icon">📊</div>
                <div class="header-title">
                    <h1>Pro Tipster</h1>
                    <p>Análisis estadístico · Fija del Día</p>
                </div>
            </div>
            <div class="header-right">
                <div class="date-badge" id="dateBadge">
                    <span class="dot" id="statusDot"></span>
                    <span id="fechaTexto">Cargando...</span>
                </div>
                <div class="header-actions">
                    <button class="btn btn-refresh" id="btnActualizar">🔄 Actualizar</button>
                </div>
            </div>
        </header>

        <div class="api-section">
            <label>🔑 <span>API Keys</span></label>
            <div class="api-input-group">
                <input type="password" id="apiKeyFootball" placeholder="API-Football Key" />
                <input type="password" id="apiKeyOdds" placeholder="The Odds API Key" value="9aba9337459f66b5ed5daa705d58fb57" />
                <button class="btn btn-primary" id="btnGuardarKeys">Guardar Keys</button>
                <button class="btn btn-secondary" id="btnAnalizar">🔍 Analizar Hoy</button>
            </div>
        </div>

        <div class="status-bar" id="statusBar">
            <span id="statusIcon">●</span>
            <span id="statusText">Ingresa tus API Keys y presiona "Analizar Hoy"</span>
        </div>

        <div class="tabs">
            <button class="tab-btn active" data-tab="tab-fija">⭐ Fija del Día</button>
            <button class="tab-btn" data-tab="tab-top5">🏆 Top 5 Goles</button>
            <button class="tab-btn" data-tab="tab-todos">📋 Todos los Partidos</button>
        </div>

        <div class="tab-content active" id="tab-fija">
            <div id="fijaContainer">
                <div class="empty-state">
                    <div class="empty-icon">📊</div>
                    <h3>Esperando análisis</h3>
                    <p>Guarda tus API Keys y presiona "Analizar Hoy" para obtener la fija del día</p>
                </div>
            </div>
        </div>

        <div class="tab-content" id="tab-top5">
            <div style="display:flex; justify-content:space-between; align-items:center; margin-bottom:14px; flex-wrap:wrap; gap:8px;">
                <h3 style="font-size:17px; font-weight:600;">📈 Mayor proyección de goles</h3>
                <span id="top5Count" style="color:#4a6078; font-size:13px;">0 partidos</span>
            </div>
            <div class="matches-grid" id="top5Grid">
                <div class="empty-state" style="grid-column:1/-1;">
                    <div class="empty-icon">⚽</div>
                    <h3>Sin datos</h3>
                    <p>Analiza los partidos para ver el Top 5</p>
                </div>
            </div>
        </div>

        <div class="tab-content" id="tab-todos">
            <div style="display:flex; justify-content:space-between; align-items:center; margin-bottom:14px; flex-wrap:wrap; gap:8px;">
                <h3 style="font-size:17px; font-weight:600;">📅 Todos los partidos de hoy</h3>
                <span id="todosCount" style="color:#4a6078; font-size:13px;">0 partidos</span>
            </div>
            <div class="matches-grid" id="todosGrid">
                <div class="empty-state" style="grid-column:1/-1;">
                    <div class="empty-icon">📅</div>
                    <h3>No hay partidos</h3>
                    <p>Los partidos del día se mostrarán aquí</p>
                </div>
            </div>
        </div>
    </div>

    <script>
        (function() {
            "use strict";

            // CONFIG
            const FOOTBALL_HOST = 'v3.football.api-sports.io';
            const FOOTBALL_URL = 'https://v3.football.api-sports.io';
            const ODDS_URL = 'https://api.the-odds-api.com/v4';

            // DOM refs
            const apiKeyFootball = document.getElementById('apiKeyFootball');
            const apiKeyOdds = document.getElementById('apiKeyOdds');
            const btnGuardarKeys = document.getElementById('btnGuardarKeys');
            const btnAnalizar = document.getElementById('btnAnalizar');
            const btnActualizar = document.getElementById('btnActualizar');
            const statusBar = document.getElementById('statusBar');
            const statusIcon = document.getElementById('statusIcon');
            const statusText = document.getElementById('statusText');
            const fechaTexto = document.getElementById('fechaTexto');
            const statusDot = document.getElementById('statusDot');

            const fijaContainer = document.getElementById('fijaContainer');
            const top5Grid = document.getElementById('top5Grid');
            const todosGrid = document.getElementById('todosGrid');
            const top5Count = document.getElementById('top5Count');
            const todosCount = document.getElementById('todosCount');

            const tabBtns = document.querySelectorAll('.tab-btn');
            const tabContents = document.querySelectorAll('.tab-content');

            // State
            let cachedFootballKey = localStorage.getItem('pro_tipster_football_key') || '';
            let cachedOddsKey = localStorage.getItem('pro_tipster_odds_key') || '9aba9337459f66b5ed5daa705d58fb57';
            let allMatches = [];
            let top5Matches = [];
            let fijaMatch = null;
            let isAnalyzing = false;
            let isRefreshing = false;
            let ultimaActualizacion = null;

            // ---------- INIT ----------
            function init() {
                if (cachedFootballKey) {
                    apiKeyFootball.value = cachedFootballKey;
                }
                if (cachedOddsKey) {
                    apiKeyOdds.value = cachedOddsKey;
                }

                const hoy = new Date();
                fechaTexto.textContent = hoy.toLocaleDateString('es-ES', {
                    weekday: 'long',
                    day: 'numeric',
                    month: 'long',
                    year: 'numeric'
                });

                btnGuardarKeys.addEventListener('click', guardarKeys);
                btnAnalizar.addEventListener('click', analizarHoy);
                btnActualizar.addEventListener('click', actualizarPartidos);

                tabBtns.forEach(btn => {
                    btn.addEventListener('click', () => {
                        tabBtns.forEach(b => b.classList.remove('active'));
                        tabContents.forEach(c => c.classList.remove('active'));
                        btn.classList.add('active');
                        document.getElementById(btn.dataset.tab).classList.add('active');
                    });
                });

                if (cachedFootballKey || cachedOddsKey) {
                    setStatus('✅ Keys cargadas. Presiona "Analizar Hoy"', false);
                }

                // Mostrar demo si no hay keys
                if (!cachedFootballKey && !cachedOddsKey) {
                    mostrarDemoData();
                }

                // Actualizar estado de partidos cada 30 segundos
                setInterval(() => {
                    if (allMatches.length > 0) {
                        actualizarResultados();
                    }
                }, 30000);
            }

            // ---------- ACTUALIZAR PARTIDOS ----------
            async function actualizarPartidos() {
                if (isRefreshing) return;

                isRefreshing = true;
                btnActualizar.disabled = true;
                btnActualizar.textContent = '⏳ Actualizando...';
                setStatus('🔄 Actualizando resultados de partidos...', false, true);

                try {
                    // Si hay datos, actualizar resultados
                    if (allMatches.length > 0) {
                        await actualizarResultados();
                        setStatus(`✅ Actualizado: ${new Date().toLocaleTimeString()} · ${allMatches.length} partidos`, false);
                        ultimaActualizacion = new Date();
                    } else {
                        // Si no hay datos, hacer un análisis completo
                        await analizarHoy();
                    }
                } catch (error) {
                    console.error(error);
                    setStatus('❌ Error al actualizar: ' + error.message, true);
                } finally {
                    isRefreshing = false;
                    btnActualizar.disabled = false;
                    btnActualizar.textContent = '🔄 Actualizar';
                }
            }

            // ---------- ACTUALIZAR RESULTADOS ----------
            async function actualizarResultados() {
                const fKey = apiKeyFootball.value.trim() || cachedFootballKey;

                if (!fKey) {
                    // Si no hay API Key, solo mostrar estado pendiente
                    allMatches.forEach(m => {
                        if (!m.resultado) {
                            m.resultado = {
                                status: 'Pendiente',
                                overCumplido: null,
                                golesLocal: null,
                                golesVisit: null
                            };
                        }
                    });
                    renderAll();
                    return;
                }

                // Obtener resultados actualizados de la API
                for (let match of allMatches) {
                    try {
                        const fixtureId = match.fixture.id;
                        if (!fixtureId) continue;

                        const url = `${FOOTBALL_URL}/fixtures?id=${fixtureId}`;
                        const response = await fetch(url, {
                            headers: {
                                'x-rapidapi-key': fKey,
                                'x-rapidapi-host': FOOTBALL_HOST
                            }
                        });

                        if (response.ok) {
                            const data = await response.json();
                            if (data.response && data.response.length > 0) {
                                const fixture = data.response[0];
                                const status = fixture.fixture.status.short;

                                if (status === 'FT' || status === 'AET' || status === 'PEN') {
                                    const golesLocal = fixture.goals.home ?? 0;
                                    const golesVisit = fixture.goals.away ?? 0;
                                    const totalGoles = golesLocal + golesVisit;
                                    const overCumplido = totalGoles >= 2.5;

                                    match.resultado = {
                                        status: 'Finalizado',
                                        overCumplido: overCumplido,
                                        golesLocal: golesLocal,
                                        golesVisit: golesVisit,
                                        totalGoles: totalGoles,
                                        ganador: golesLocal > golesVisit ? 'local' :
                                            golesVisit > golesLocal ? 'visitante' : 'empate'
                                    };
                                } else {
                                    match.resultado = {
                                        status: 'Pendiente',
                                        overCumplido: null,
                                        golesLocal: null,
                                        golesVisit: null
                                    };
                                }
                            }
                        }
                    } catch (e) {
                        console.warn('Error obteniendo resultado para', match.fixture.id, e);
                    }

                    // Pequeña pausa para no saturar la API
                    await sleep(100);
                }

                renderAll();
            }

            // ---------- GUARDAR KEYS ----------
            function guardarKeys() {
                const fKey = apiKeyFootball.value.trim();
                const oKey = apiKeyOdds.value.trim();

                if (fKey) {
                    localStorage.setItem('pro_tipster_football_key', fKey);
                    cachedFootballKey = fKey;
                }
                if (oKey) {
                    localStorage.setItem('pro_tipster_odds_key', oKey);
                    cachedOddsKey = oKey;
                }

                if (!fKey && !oKey) {
                    setStatus('⚠️ Ingresa al menos una API Key', true);
                    return;
                }

                setStatus('✅ Keys guardadas correctamente', false);
            }

            // ---------- SET STATUS ----------
            function setStatus(msg, isError = false, isLoading = false) {
                statusText.textContent = msg;
                statusBar.className = 'status-bar';
                if (isError) statusBar.classList.add('error');
                if (isLoading) {
                    statusIcon.innerHTML = '<span class="spinner"></span>';
                } else {
                    statusIcon.textContent = isError ? '✖' : '●';
                }
            }

            // ---------- MOSTRAR DEMO ----------
            function mostrarDemoData() {
                const hoy = new Date();
                const fechaStr = hoy.getFullYear() + '-' +
                    String(hoy.getMonth() + 1).padStart(2, '0') + '-' +
                    String(hoy.getDate()).padStart(2, '0');

                const demoMatches = [{
                    fixture: { date: fechaStr + 'T22:00:00Z', id: 'demo1' },
                    league: { name: 'Copa Libertadores', id: 'LIB' },
                    teams: {
                        home: { name: 'Flamengo', id: 'fla' },
                        away: { name: 'Palmeiras', id: 'pal' }
                    },
                    goals: { home: 2, away: 1 }
                }, {
                    fixture: { date: fechaStr + 'T20:30:00Z', id: 'demo2' },
                    league: { name: 'Copa Sudamericana', id: 'SUD' },
                    teams: {
                        home: { name: 'River Plate', id: 'riv' },
                        away: { name: 'Boca Juniors', id: 'boc' }
                    },
                    goals: { home: 1, away: 1 }
                }, {
                    fixture: { date: fechaStr + 'T23:00:00Z', id: 'demo3' },
                    league: { name: 'Brasileirão', id: 'BRA' },
                    teams: {
                        home: { name: 'Corinthians', id: 'cor' },
                        away: { name: 'São Paulo', id: 'spa' }
                    },
                    goals: { home: null, away: null }
                }, {
                    fixture: { date: fechaStr + 'T01:00:00Z', id: 'demo4' },
                    league: { name: 'Primera División Argentina', id: 'ARG' },
                    teams: {
                        home: { name: 'Racing Club', id: 'rac' },
                        away: { name: 'Independiente', id: 'ind' }
                    },
                    goals: { home: 3, away: 0 }
                }, {
                    fixture: { date: fechaStr + 'T02:30:00Z', id: 'demo5' },
                    league: { name: 'Primera División Chile', id: 'CHI' },
                    teams: {
                        home: { name: 'Colo-Colo', id: 'col' },
                        away: { name: 'Universidad de Chile', id: 'uch' }
                    },
                    goals: { home: 0, away: 2 }
                }];

                const analyzed = demoMatches.map((f, index) => {
                    const statsLocal = {
                        promedioFavor: 1.8 + (index * 0.2),
                        promedioContra: 0.8 + (index * 0.1),
                        diferencia: 1.0 + (index * 0.1),
                        partidos: 5,
                        estimado: true
                    };
                    const statsVisit = {
                        promedioFavor: 1.3 + (index * 0.15),
                        promedioContra: 1.2 + (index * 0.1),
                        diferencia: 0.1 + (index * 0.05),
                        partidos: 5,
                        estimado: true
                    };

                    const promedioGoles = (statsLocal.promedioFavor + statsLocal.promedioContra +
                        statsVisit.promedioFavor + statsVisit.promedioContra) / 4;

                    // Determinar resultado
                    let resultado = null;
                    if (f.goals.home !== null && f.goals.away !== null) {
                        const totalGoles = f.goals.home + f.goals.away;
                        resultado = {
                            status: 'Finalizado',
                            overCumplido: totalGoles >= 2.5,
                            golesLocal: f.goals.home,
                            golesVisit: f.goals.away,
                            totalGoles: totalGoles,
                            ganador: f.goals.home > f.goals.away ? 'local' :
                                f.goals.away > f.goals.home ? 'visitante' : 'empate'
                        };
                    } else {
                        resultado = {
                            status: 'Pendiente',
                            overCumplido: null,
                            golesLocal: null,
                            golesVisit: null
                        };
                    }

                    return {
                        fixture: f,
                        promedioGoles: promedioGoles,
                        indiceGoles: 1.5 + (index * 0.3) + (Math.random() * 0.2),
                        ganador: index % 2 === 0 ? f.teams.home.name : 'Empate',
                        probLocal: 40 + (index * 2),
                        probVisit: 30 + (index * 1.5),
                        probEmpate: 30 - (index * 1.5),
                        statsLocal: statsLocal,
                        statsVisit: statsVisit,
                        diffGlobal: statsLocal.diferencia - statsVisit.diferencia,
                        over15: 70 + (index * 2),
                        over25: 45 + (index * 3),
                        datosReales: false,
                        resultado: resultado
                    };
                });

                allMatches = analyzed;
                top5Matches = analyzed.slice(0, 5);
                fijaMatch = top5Matches[0];

                renderAll();
                setStatus('📊 Mostrando datos de demostración (sin API Key)', false);
            }

            // ---------- ANALIZAR HOY ----------
            async function analizarHoy() {
                if (isAnalyzing) return;

                const fKey = apiKeyFootball.value.trim() || cachedFootballKey;
                const oKey = apiKeyOdds.value.trim() || cachedOddsKey;

                if (!fKey && !oKey) {
                    setStatus('⚠️ Ingresa al menos una API Key o usa datos de demostración', true);
                    mostrarDemoData();
                    return;
                }

                if (fKey && fKey !== cachedFootballKey) {
                    localStorage.setItem('pro_tipster_football_key', fKey);
                    cachedFootballKey = fKey;
                }
                if (oKey && oKey !== cachedOddsKey) {
                    localStorage.setItem('pro_tipster_odds_key', oKey);
                    cachedOddsKey = oKey;
                }

                isAnalyzing = true;
                btnAnalizar.disabled = true;
                btnAnalizar.textContent = '⏳ Analizando...';
                setStatus('🔄 Obteniendo partidos de hoy...', false, true);

                try {
                    const hoy = new Date();
                    const fechaStr = hoy.getFullYear() + '-' +
                        String(hoy.getMonth() + 1).padStart(2, '0') + '-' +
                        String(hoy.getDate()).padStart(2, '0');

                    console.log('📅 Buscando partidos para:', fechaStr);

                    let fixtures = [];

                    if (fKey) {
                        try {
                            const footballFixtures = await fetchFixturesFootball(fechaStr, fKey);
                            if (footballFixtures && footballFixtures.length > 0) {
                                fixtures = footballFixtures;
                                console.log(`⚽ Football API: ${fixtures.length} partidos`);
                            }
                        } catch (e) {
                            console.warn('Football API error:', e);
                            setStatus(`⚠️ Football API: ${e.message}`, true);
                        }
                    }

                    if (fixtures.length === 0 && oKey) {
                        try {
                            const oddsData = await fetchOdds(oKey);
                            if (oddsData && oddsData.length > 0) {
                                fixtures = convertOddsToFixtures(oddsData);
                                console.log(`🎲 Odds API: ${fixtures.length} partidos`);
                            }
                        } catch (e) {
                            console.warn('Odds API error:', e);
                            setStatus(`⚠️ Odds API: ${e.message}`, true);
                        }
                    }

                    if (fixtures.length === 0) {
                        setStatus('⚠️ No se encontraron partidos. Usando datos de demostración.', true);
                        mostrarDemoData();
                        return;
                    }

                    const hoyPartidos = fixtures.filter(f => {
                        const fDate = f.fixture?.date ? f.fixture.date.split('T')[0] : '';
                        return fDate === fechaStr;
                    });

                    console.log(`📊 Total: ${fixtures.length} | Hoy: ${hoyPartidos.length}`);

                    if (hoyPartidos.length === 0) {
                        setStatus(`⚠️ No hay partidos para hoy (${fechaStr})`, true);
                        mostrarDemoData();
                        return;
                    }

                    setStatus(`📋 ${hoyPartidos.length} partidos encontrados. Analizando...`, false, true);

                    const analyzed = await analizarTodosPartidos(hoyPartidos, fKey, oKey);

                    analyzed.sort((a, b) => b.indiceGoles - a.indiceGoles);

                    allMatches = analyzed;
                    top5Matches = analyzed.slice(0, Math.min(5, analyzed.length));
                    fijaMatch = top5Matches.length > 0 ? top5Matches[0] : null;

                    // Verificar resultados iniciales
                    await actualizarResultados();

                    const fuente = fKey ? 'Football API' : 'Odds API';
                    setStatus(`✅ Análisis completado · ${analyzed.length} partidos (${fuente})`, false);
                    ultimaActualizacion = new Date();

                } catch (error) {
                    console.error(error);
                    setStatus('❌ ' + error.message, true);
                    mostrarVacio();
                } finally {
                    isAnalyzing = false;
                    btnAnalizar.disabled = false;
                    btnAnalizar.textContent = '🔍 Analizar Hoy';
                }
            }

            // ---------- FETCH FUNCTIONS ----------
            async function fetchFixturesFootball(date, apiKey) {
                const url = `${FOOTBALL_URL}/fixtures?date=${date}`;
                const response = await fetch(url, {
                    headers: {
                        'x-rapidapi-key': apiKey,
                        'x-rapidapi-host': FOOTBALL_HOST
                    }
                });

                if (!response.ok) {
                    let msg = `Error ${response.status}`;
                    if (response.status === 429) msg = 'Límite de peticiones (429). Espera o cambia de API Key.';
                    else if (response.status === 403) msg = 'API Key inválida o expirada.';
                    throw new Error(msg);
                }

                const data = await response.json();
                return data.response || [];
            }

            async function fetchOdds(apiKey) {
                const sportsUrl = `${ODDS_URL}/sports?apiKey=${apiKey}`;

                let sportsResponse = await fetch(sportsUrl);
                if (!sportsResponse.ok) {
                    throw new Error(`Error obteniendo deportes: ${sportsResponse.status}`);
                }

                const sports = await sportsResponse.json();

                const targetSports = sports.filter(s =>
                    s.key.includes('soccer') &&
                    (s.key.includes('brazil') ||
                        s.key.includes('argentina') ||
                        s.key.includes('chile') ||
                        s.key.includes('uruguay') ||
                        s.key.includes('peru') ||
                        s.key.includes('ecuador') ||
                        s.key.includes('colombia') ||
                        s.key.includes('libertadores') ||
                        s.key.includes('sudamericana') ||
                        s.key.includes('spain') ||
                        s.key.includes('england') ||
                        s.key.includes('italy') ||
                        s.key.includes('germany') ||
                        s.key.includes('france'))
                );

                if (targetSports.length === 0) {
                    const soccerSports = sports.filter(s => s.key.includes('soccer'));
                    if (soccerSports.length === 0) {
                        throw new Error('No se encontraron deportes de fútbol');
                    }
                    targetSports.push(soccerSports[0]);
                }

                let allOdds = [];
                for (const sport of targetSports) {
                    const oddsUrl =
                        `${ODDS_URL}/sports/${sport.key}/odds/?apiKey=${apiKey}&regions=eu&markets=h2h,totals`;

                    const oddsResponse = await fetch(oddsUrl);
                    if (oddsResponse.ok) {
                        const data = await oddsResponse.json();
                        allOdds = allOdds.concat(data);
                    }
                }

                if (allOdds.length === 0) {
                    throw new Error('No se encontraron cuotas disponibles');
                }

                return allOdds;
            }

            function convertOddsToFixtures(oddsData) {
                return oddsData.map(odd => {
                    const homeTeam = odd.home_team || 'Local';
                    const awayTeam = odd.away_team || 'Visitante';
                    const commenceTime = odd.commence_time || new Date().toISOString();

                    const leagueName = odd.sport_title || 'Liga';
                    const leagueKey = odd.sport_key || 'unknown';

                    return {
                        fixture: {
                            date: commenceTime,
                            id: odd.id || Math.random().toString(36).substr(2, 9)
                        },
                        league: {
                            name: leagueName,
                            id: leagueKey,
                            key: leagueKey
                        },
                        teams: {
                            home: { name: homeTeam, id: 'home_' + homeTeam.replace(/\s/g, '') },
                            away: { name: awayTeam, id: 'away_' + awayTeam.replace(/\s/g, '') }
                        },
                        goals: { home: null, away: null },
                        odds: odd,
                        source: 'odds'
                    };
                });
            }

            // ---------- TEAM STATS ----------
            async function fetchTeamStats(teamId, leagueId, apiKey, season = 2025) {
                if (!apiKey) return [];

                const seasons = [2025, 2024, 2023];
                let allMatches = [];

                for (const s of seasons) {
                    try {
                        const url =
                            `${FOOTBALL_URL}/fixtures?team=${teamId}&league=${leagueId}&season=${s}&last=10`;
                        const response = await fetch(url, {
                            headers: {
                                'x-rapidapi-key': apiKey,
                                'x-rapidapi-host': FOOTBALL_HOST
                            }
                        });
                        if (response.ok) {
                            const data = await response.json();
                            if (data.response && data.response.length > 0) {
                                allMatches = allMatches.concat(data.response);
                                if (allMatches.length >= 10) break;
                            }
                        }
                    } catch (e) {
                        console.warn(`Season ${s} failed for team ${teamId}:`, e);
                    }
                }

                return allMatches;
            }

            function calcularMetricas(partidos, teamId) {
                if (!partidos || partidos.length === 0) {
                    return {
                        promedioFavor: 1.2 + (Math.random() * 0.8),
                        promedioContra: 1.0 + (Math.random() * 0.6),
                        diferencia: 0.2 + (Math.random() * 0.4),
                        totalGoles: 2.2 + (Math.random() * 1.2),
                        partidos: 5,
                        golesFavor: 6 + Math.floor(Math.random() * 4),
                        golesContra: 5 + Math.floor(Math.random() * 3),
                        estimado: true
                    };
                }

                let golesFavor = 0,
                    golesContra = 0;
                let partidosJugados = 0;

                for (let p of partidos) {
                    if (!p.goals) continue;
                    const local = p.teams.home.id === teamId;
                    const golesLocal = p.goals.home ?? 0;
                    const golesVisit = p.goals.away ?? 0;
                    if (local) {
                        golesFavor += golesLocal;
                        golesContra += golesVisit;
                    } else {
                        golesFavor += golesVisit;
                        golesContra += golesLocal;
                    }
                    partidosJugados++;
                }

                const total = partidosJugados || 1;
                return {
                    promedioFavor: golesFavor / total,
                    promedioContra: golesContra / total,
                    diferencia: (golesFavor - golesContra) / total,
                    totalGoles: golesFavor + golesContra,
                    partidos: total,
                    golesFavor: golesFavor,
                    golesContra: golesContra,
                    estimado: false
                };
            }

            // ---------- ANALYZE ----------
            function analizarPartido(fixture, statsLocal, statsVisit, odds = null) {
                const promTotalLocal = statsLocal.promedioFavor + statsLocal.promedioContra;
                const promTotalVisit = statsVisit.promedioFavor + statsVisit.promedioContra;
                const promedioGoles = (promTotalLocal + promTotalVisit) / 2;

                const diffLocal = statsLocal.promedioFavor - statsLocal.promedioContra;
                const diffVisit = statsVisit.promedioFavor - statsVisit.promedioContra;
                const diffGlobal = diffLocal - diffVisit;

                let probLocal = 0.35 + (diffGlobal * 0.10) + 0.05;
                let probVisit = 0.35 - (diffGlobal * 0.10);
                let probEmpate = 0.30;

                if (odds && odds.home_win && odds.away_win && odds.draw) {
                    const totalOdds = (1 / odds.home_win) + (1 / odds.away_win) + (1 / odds.draw);
                    probLocal = (1 / odds.home_win) / totalOdds;
                    probVisit = (1 / odds.away_win) / totalOdds;
                    probEmpate = (1 / odds.draw) / totalOdds;
                }

                const totalProb = probLocal + probVisit + probEmpate;
                probLocal = Math.round((probLocal / totalProb) * 100);
                probVisit = Math.round((probVisit / totalProb) * 100);
                probEmpate = Math.round((probEmpate / totalProb) * 100);

                let ganador = 'Empate';
                let maxProb = probEmpate;
                if (probLocal > maxProb) { ganador = fixture.teams.home.name;
                    maxProb = probLocal; }
                if (probVisit > maxProb) { ganador = fixture.teams.away.name;
                    maxProb = probVisit; }

                const indiceGoles = (promedioGoles * 1.3) + (Math.abs(diffGlobal) * 0.4) +
                    (statsLocal.estimado || statsVisit.estimado ? 0.2 : 0);

                let over15 = 60 + (promedioGoles * 10);
                let over25 = 30 + (promedioGoles * 12);

                if (odds && odds.totals) {
                    const totalLine = odds.totals.point || 2.5;
                    const totalProb = odds.totals.over || 0.5;
                    over25 = Math.round(totalProb * 100);
                    over15 = Math.min(95, over25 + 25);
                }

                return {
                    fixture,
                    promedioGoles,
                    indiceGoles,
                    ganador,
                    probLocal,
                    probVisit,
                    probEmpate,
                    statsLocal,
                    statsVisit,
                    diffGlobal,
                    over15: Math.min(95, Math.round(over15)),
                    over25: Math.min(92, Math.round(over25)),
                    datosReales: !statsLocal.estimado && !statsVisit.estimado,
                    resultado: null
                };
            }

            async function analizarTodosPartidos(fixtures, footballKey, oddsKey) {
                const resultados = [];
                let procesados = 0;

                let oddsMap = {};
                if (oddsKey) {
                    try {
                        const oddsData = await fetchOdds(oddsKey);
                        oddsData.forEach(o => {
                            const key = `${o.home_team}_vs_${o.away_team}`;
                            oddsMap[key] = o;
                        });
                    } catch (e) {
                        console.warn('No se pudieron obtener odds:', e);
                    }
                }

                for (let f of fixtures) {
                    const localId = f.teams.home.id;
                    const visitId = f.teams.away.id;
                    const ligaId = f.league?.id || f.league?.key || 'unknown';

                    let partidosLocal = [];
                    let partidosVisit = [];

                    if (footballKey && ligaId !== 'unknown') {
                        partidosLocal = await fetchTeamStats(localId, ligaId, footballKey);
                        partidosVisit = await fetchTeamStats(visitId, ligaId, footballKey);
                    }

                    const statsLocal = calcularMetricas(partidosLocal, localId);
                    const statsVisit = calcularMetricas(partidosVisit, visitId);

                    const key = `${f.teams.home.name}_vs_${f.teams.away.name}`;
                    const odds = oddsMap[key] || null;

                    const analisis = analizarPartido(f, statsLocal, statsVisit, odds);
                    resultados.push(analisis);

                    procesados++;
                    if (procesados % 3 === 0) {
                        setStatus(`📊 Procesando ${procesados}/${fixtures.length}...`, false, true);
                    }

                    await sleep(100);
                }

                return resultados;
            }

            function sleep(ms) { return new Promise(r => setTimeout(r, ms)); }

            // ---------- RENDER FUNCTIONS ----------
            function renderAll() {
                renderTodos(allMatches);
                renderTop5(top5Matches);
                renderFija(fijaMatch);
                updateCounts();
                updateStatusDot();
            }

            function updateCounts() {
                todosCount.textContent = `${allMatches.length} partidos`;
                top5Count.textContent = `${top5Matches.length} partidos`;
            }

            function updateStatusDot() {
                const dot = document.getElementById('statusDot');
                if (!dot) return;

                const hayPendientes = allMatches.some(m => !m.resultado || m.resultado.status === 'Pendiente');
                const hayFinalizados = allMatches.some(m => m.resultado && m.resultado.status === 'Finalizado');

                if (hayFinalizados && !hayPendientes) {
                    dot.className = 'dot finished';
                } else if (hayPendientes) {
                    dot.className = 'dot pending';
                } else {
                    dot.className = 'dot';
                }
            }

            function renderTodos(matches) {
                if (!matches || matches.length === 0) {
                    todosGrid.innerHTML =
                        `<div class="empty-state" style="grid-column:1/-1;"><div class="empty-icon">📅</div><h3>No hay partidos</h3><p>Los partidos del día se mostrarán aquí</p></div>`;
                    return;
                }

                let html = '';
                for (let item of matches) {
                    html += renderMatchCard(item, false);
                }
                todosGrid.innerHTML = html;
            }

            function renderTop5(matches) {
                if (!matches || matches.length === 0) {
                    top5Grid.innerHTML =
                        `<div class="empty-state" style="grid-column:1/-1;"><div class="empty-icon">⚽</div><h3>Sin datos</h3><p>Analiza los partidos para ver el Top 5</p></div>`;
                    return;
                }

                let html = '';
                for (let i = 0; i < matches.length; i++) {
                    const item = matches[i];
                    const isTop = i === 0;
                    html += renderMatchCard(item, isTop, i + 1);
                }
                top5Grid.innerHTML = html;
            }

            function getResultBadge(resultado) {
                if (!resultado || resultado.status === 'Pendiente') {
                    return '<span class="result-badge result-pending">⏳ Pendiente</span>';
                }

                if (resultado.overCumplido === true) {
                    return `<span class="result-badge result-over">✅ Over ${resultado.totalGoles} goles</span>`;
                } else if (resultado.overCumplido === false) {
                    return `<span class="result-badge result-under">❌ Under ${resultado.totalGoles} goles</span>`;
                }

                if (resultado.ganador === 'local') {
                    return `<span class="result-badge result-win">🏠 ${resultado.golesLocal}-${resultado.golesVisit}</span>`;
                } else if (resultado.ganador === 'visitante') {
                    return `<span class="result-badge result-loss">✈️ ${resultado.golesLocal}-${resultado.golesVisit}</span>`;
                } else if (resultado.ganador === 'empate') {
                    return `<span class="result-badge result-draw">🤝 ${resultado.golesLocal}-${resultado.golesVisit}</span>`;
                }

                return '';
            }

            function renderMatchCard(item, isTop = false, rank = 0) {
                const f = item.fixture;
                const local = f.teams.home.name;
                const visit = f.teams.away.name;
                const liga = f.league?.name || 'Liga';
                const time = f.fixture?.date ? new Date(f.fixture.date).toLocaleTimeString('es-ES', {
                    hour: '2-digit',
                    minute: '2-digit'
                }) : '--:--';

                const statsL = item.statsLocal;
                const statsV = item.statsVisit;
                const isFija = isTop && rank === 1;

                const datosReales = item.datosReales ? '✅ Datos reales' : '📊 Estimado';
                const resultadoBadge = getResultBadge(item.resultado);

                return `
                    <div class="match-card ${isFija ? 'top-match' : ''}">
                        ${isFija ? '<span class="badge-top">⭐ FIJA</span>' : ''}
                        ${isTop && !isFija ? `<span class="badge-top" style="background:#4a6078;">#${rank}</span>` : ''}

                        <div class="match-header">
                            <span class="league">${liga}</span>
                            <span class="time">🕐 ${time}</span>
                            <span class="data-source-badge">${datosReales}</span>
                        </div>

                        <div class="teams">
                            <span class="team">
                                <span class="flag">🏠</span>
                                ${local}
                                ${resultadoBadge}
                            </span>
                            <span class="vs">vs</span>
                            <span class="team away">
                                ${visit}
                                <span class="flag">✈️</span>
                            </span>
                        </div>

                        <div class="tipster-stats">
                            <div class="stat">
                                <span class="label">⚽ Prom. Goles</span>
                                <span class="value gold">${item.promedioGoles.toFixed(2)}</span>
                            </div>
                            <div class="stat">
                                <span class="label">📊 Índice Gol</span>
                                <span class="value ${isFija ? 'gold' : ''}">${item.indiceGoles.toFixed(1)}</span>
                            </div>
                            <div class="stat">
                                <span class="label">📈 Over 2.5</span>
                                <span class="value green">${item.over25}%</span>
                            </div>
                            <div class="stat">
                                <span class="label">🏠 ${local}</span>
                                <span class="value">${statsL.promedioFavor.toFixed(1)} GF</span>
                            </div>
                            <div class="stat">
                                <span class="label">✈️ ${visit}</span>
                                <span class="value">${statsV.promedioFavor.toFixed(1)} GF</span>
                            </div>
                            <div class="stat">
                                <span class="label">🛡️ Defensa</span>
                                <span class="value red">${statsL.promedioContra.toFixed(1)} - ${statsV.promedioContra.toFixed(1)}</span>
                            </div>
                        </div>

                        <div class="prob-bar">
                            <div class="bar-local" style="width:${item.probLocal}%;"></div>
                            <div class="bar-draw" style="width:${item.probEmpate}%;"></div>
                            <div class="bar-away" style="width:${item.probVisit}%;"></div>
                        </div>
                        <div class="prob-labels">
                            <span>🏠 ${item.probLocal}%</span>
                            <span>🤝 ${item.probEmpate}%</span>
                            <span>✈️ ${item.probVisit}%</span>
                        </div>

                        <div class="prediction-line">
                            <span>🎯 <span class="favorite">${item.ganador}</span> (favorito)</span>
                            <span>⚡ Over 1.5: <span class="over-under">${item.over15}%</span></span>
                        </div>
                    </div>
                `;
            }

            function renderFija(match) {
                if (!match) {
                    fijaContainer.innerHTML = `
                        <div class="empty-state">
                            <div class="empty-icon">📊</div>
                            <h3>Esperando análisis</h3>
                            <p>Guarda tus API Keys y presiona "Analizar Hoy" para obtener la fija del día</p>
                        </div>
                    `;
                    return;
                }

                const f = match.fixture;
                const local = f.teams.home.name;
                const visit = f.teams.away.name;
                const liga = f.league?.name || 'Liga';
                const time = f.fixture?.date ? new Date(f.fixture.date).toLocaleTimeString('es-ES', {
                    hour: '2-digit',
                    minute: '2-digit'
                }) : '--:--';

                const statsL = match.statsLocal;
                const statsV = match.statsVisit;

                const probGanador = match.ganador === 'Empate' ? match.probEmpate :
                    (match.ganador === local ? match.probLocal : match.probVisit);

                const datosReales = match.datosReales ? '✅ Datos reales' : '📊 Estimado';
                const resultadoBadge = getResultBadge(match.resultado);

                const args = [
                    `⚽ ${local} promedia ${statsL.promedioFavor.toFixed(1)} GF y ${statsL.promedioContra.toFixed(1)} GC en ${statsL.partidos} partidos. ${visit} tiene ${statsV.promedioFavor.toFixed(1)} GF y ${statsV.promedioContra.toFixed(1)} GC. Diferencia de gol: ${(statsL.promedioFavor - statsL.promedioContra).toFixed(2)} vs ${(statsV.promedioFavor - statsV.promedioContra).toFixed(2)}. ${statsL.estimado ? '(Datos estimados)' : ''}`,
                    `📈 Promedio combinado de goles: ${match.promedioGoles.toFixed(2)} por partido. Índice de gol: ${match.indiceGoles.toFixed(1)} (el más alto del día). Probabilidad de Over 2.5: ${match.over25}%.`,
                    `🎯 El equipo con mayor probabilidad de ganar es ${match.ganador} con ${probGanador}% según diferencial de rendimiento y factor localía. ${match.ganador === 'Empate' ? 'Ambos equipos muestran balance similar.' : 'Su diferencia de gol (+${(statsL.promedioFavor - statsL.promedioContra).toFixed(2)} vs ${(statsV.promedioFavor - statsV.promedioContra).toFixed(2)}) respalda la confianza.'}`
                ];

                fijaContainer.innerHTML = `
                    <div class="fija-section">
                        <div class="fija-header">
                            <span class="fija-icon">⭐</span>
                            <h2>Fija del Día</h2>
                            <span class="fija-badge">${liga}</span>
                            <span style="margin-left:auto; font-size:12px; color:#4a6078;">🕐 ${time}</span>
                            <span class="data-source-badge">${datosReales}</span>
                        </div>

                        <div class="fija-match">
                            <span class="team-name local">${local}</span>
                            <span class="vs-text">vs</span>
                            <span class="team-name">${visit}</span>
                            <span class="prediction">🎯 ${match.ganador} (${probGanador}%)</span>
                            ${resultadoBadge}
                        </div>

                        <div class="fija-stats-grid">
                            <div class="fs-item">
                                <span class="fs-label">⚽ Prom. Goles</span>
                                <span class="fs-value gold">${match.promedioGoles.toFixed(2)}</span>
                            </div>
                            <div class="fs-item">
                                <span class="fs-label">📊 Índice de Gol</span>
                                <span class="fs-value gold">${match.indiceGoles.toFixed(1)}</span>
                            </div>
                            <div class="fs-item">
                                <span class="fs-label">📈 Over 2.5</span>
                                <span class="fs-value green">${match.over25}%</span>
                            </div>
                            <div class="fs-item">
                                <span class="fs-label">🏠 ${local}</span>
                                <span class="fs-value">${statsL.promedioFavor.toFixed(1)} GF</span>
                            </div>
                            <div class="fs-item">
                                <span class="fs-label">✈️ ${visit}</span>
                                <span class="fs-value">${statsV.promedioFavor.toFixed(1)} GF</span>
                            </div>
                            <div class="fs-item">
                                <span class="fs-label">🛡️ Diferencial</span>
                                <span class="fs-value ${statsL.promedioFavor > statsV.promedioFavor ? 'green' : 'red'}">${(statsL.promedioFavor - statsV.promedioFavor).toFixed(2)}</span>
                            </div>
                        </div>

                        <div class="fija-args">
                            ${args.map((arg, i) => `
                                <div class="arg-item">
                                    <div class="arg-num">Argumento ${i+1}</div>
                                    <div class="arg-text">${arg}</div>
                                </div>
                            `).join('')}
                        </div>
                    </div>
                `;
            }

            function mostrarVacio() {
                allMatches = [];
                top5Matches = [];
                fijaMatch = null;
                renderTodos([]);
                renderTop5([]);
                renderFija(null);
                todosCount.textContent = '0 partidos';
                top5Count.textContent = '0 partidos';
            }

            // ---------- EJECUTAR ----------
            init();

            if (cachedFootballKey || cachedOddsKey) {
                setStatus('✅ Keys cargadas. Presiona "Analizar Hoy"', false);
            }

        })();
    </script>

</body>
</html>
