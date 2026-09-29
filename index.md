<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Pro Tipster - Goles y Córners</title>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&display=swap" rel="stylesheet">
<style>
*{box-sizing:border-box}
body{margin:0;padding:16px;background:#0a0e1a;color:#e8edf5;font-family:Inter,Arial,sans-serif}
.app-container{max-width:1440px;margin:auto}
.header,.api-section,.match-card,.fija-section{background:linear-gradient(135deg,#111827,#1a2332);border:1px solid #273449}
.header{border-radius:20px;padding:24px 32px;margin-bottom:24px;display:flex;justify-content:space-between;align-items:center;gap:16px;flex-wrap:wrap}
.header-left,.header-right,.header-actions{display:flex;align-items:center;gap:14px;flex-wrap:wrap}
.logo-icon{width:48px;height:48px;border-radius:14px;background:linear-gradient(135deg,#f7c948,#f5a623);display:grid;place-items:center;font-size:24px}
h1{margin:0;color:#f7c948;font-size:24px}
.header-title p{margin:3px 0 0;color:#8ba0b8;font-size:12px}
.date-badge{background:#1e2d42;border:1px solid #2a4058;border-radius:60px;padding:9px 18px;color:#b6d0e8;font-size:13px}
.dot{display:inline-block;width:8px;height:8px;border-radius:50%;background:#4ade80;margin-right:8px}
.dot.pending{background:#60a5fa}
.api-section{border-radius:14px;padding:16px 24px;margin-bottom:18px;display:flex;align-items:center;gap:14px;flex-wrap:wrap}
.api-section label{font-size:13px;color:#b6d0e8;font-weight:600}
.api-input-group{display:flex;gap:10px;flex:1;flex-wrap:wrap}
input,select{background:#0d1624;border:1px solid #2a4058;border-radius:10px;padding:10px 12px;color:#e8edf5;font:inherit;font-size:13px}
input{min-width:160px;flex:1}
.btn{border:0;border-radius:10px;padding:10px 20px;font:700 13px Inter,Arial,sans-serif;cursor:pointer}
.btn:disabled{opacity:.5;cursor:wait}
.btn-primary,.tab-btn.active{background:linear-gradient(135deg,#f7c948,#f5a623);color:#0a0e1a}
.btn-secondary{background:#1e2d42;color:#b6d0e8;border:1px solid #2a4058}
.btn-refresh{background:#2563eb;color:white}
.help{color:#8ba0b8;font-size:12px;line-height:1.5;margin:0 0 18px}
.status-bar{background:#111827;border-left:4px solid #4ade80;border-radius:10px;padding:12px 18px;margin-bottom:20px;font-size:13px;display:flex;gap:10px}
.status-bar.error{border-left-color:#ef4444;color:#fca5a5}
.tabs{display:flex;flex-wrap:wrap;gap:6px;margin-bottom:20px}
.tab-btn{background:#111827;color:#8ba0b8;border:0;border-radius:10px;padding:11px 18px;font:600 13px Inter,Arial,sans-serif;cursor:pointer}
.tab-content{display:none}
.tab-content.active{display:block}
.matches-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(340px,1fr));gap:16px;margin-bottom:24px}
.match-card{border-radius:14px;padding:17px;position:relative}
.match-card.top-match,.fija-section{border-color:#f7c948}
.badge-top{position:absolute;top:10px;right:12px;background:#f7c948;color:#0a0e1a;border-radius:20px;padding:3px 12px;font-size:10px;font-weight:800}
.match-header,.teams,.prediction-line{display:flex;align-items:center;justify-content:space-between;gap:8px;flex-wrap:wrap}
.match-header{color:#8ba0b8;font-size:11px;margin-bottom:13px;padding-right:45px}
.league,.data-source-badge{background:#0d1624;padding:3px 9px;border-radius:20px}
.teams{font-size:15px;font-weight:700;margin-bottom:14px}
.vs{color:#60758d;font-size:11px}
.tipster-stats,.fija-stats-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:7px;background:#0d1624;border-radius:10px;padding:12px}
.stat,.fs-item{text-align:center}
.stat .label,.fs-label{display:block;font-size:10px;color:#6a829b;text-transform:uppercase}
.stat .value,.fs-value{display:block;font-size:15px;font-weight:700;color:#f7c948;margin-top:4px}
.prediction-line{border-top:1px solid #2a4058;margin-top:12px;padding-top:12px;font-size:12px}
.favorite{color:#f7c948;font-weight:700}
.over-under{color:#4ade80;font-weight:700}
.fija-section{border-width:2px;border-style:solid;border-radius:20px;padding:26px 30px;margin-bottom:24px}
.fija-header,.fija-match{display:flex;align-items:center;gap:14px;flex-wrap:wrap}
.fija-header h2{color:#f7c948;font-size:20px}
.fija-badge{background:#f7c948;color:#0a0e1a;padding:4px 12px;border-radius:20px;font-size:11px;font-weight:700}
.fija-match{margin:18px 0}
.team-name{font-size:20px;font-weight:700}
.team-name.local{color:#f7c948}
.vs-text{color:#60758d}
.prediction{background:#0d1624;color:#4ade80;padding:6px 15px;border-radius:25px;font-size:13px;font-weight:700}
.fija-args{display:grid;grid-template-columns:repeat(auto-fit,minmax(230px,1fr));gap:12px;margin-top:14px}
.arg-item{background:#0d1624;border-left:3px solid #f7c948;border-radius:10px;padding:12px 16px}
.arg-num{font-size:10px;color:#f7c948;font-weight:700;text-transform:uppercase}
.arg-text{font-size:12px;color:#b6d0e8;line-height:1.5;margin-top:5px}
.empty-state{text-align:center;padding:45px 20px;color:#8ba0b8}
.empty-icon{font-size:35px}
.empty-state h3{font-size:17px}
@media(max-width:700px){
 .header,.api-section{padding:16px}
 .api-input-group{flex-direction:column}
 .matches-grid{grid-template-columns:1fr}
 .tipster-stats,.fija-stats-grid{grid-template-columns:repeat(2,1fr)}
 .fija-section{padding:18px}
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
                <p>Análisis real · goles y córners</p>
            </div>
        </div>
        <div class="header-right">
            <div class="date-badge">
                <span class="dot" id="statusDot"></span>
                <span id="fechaTexto">Cargando...</span>
            </div>
            <div class="header-actions">
                <button class="btn btn-refresh" id="btnActualizar">🔄 Actualizar</button>
            </div>
        </div>
    </header>

    <div class="api-section">
        <label>🔑 API Keys: Odds obligatoria, Football para historial</label>
        <div class="api-input-group">
            <input type="password" id="apiKeyFootball" placeholder="API-Football Key">
            <input type="password" id="apiKeyOdds" placeholder="The Odds API Key">
            <select id="region" title="Región de las casas">
                <option value="eu">Europa</option>
                <option value="uk">Reino Unido</option>
                <option value="us">Estados Unidos</option>
                <option value="au">Australia</option>
            </select>
            <select id="limit" title="Máximo de partidos a consultar">
                <option value="20">20 partidos</option>
                <option value="40" selected>40 partidos</option>
                <option value="60">60 partidos</option>
                <option value="100">100 partidos</option>
            </select>
            <button class="btn btn-primary" id="btnGuardarKeys">Guardar Keys</button>
            <button class="btn btn-secondary" id="btnAnalizar">🔍 Analizar Hoy</button>
        </div>
    </div>

    <p class="help">
        Horario de Perú. La región selecciona las casas de apuestas, no el país de los partidos.
        Las cuotas se consultan hasta el límite elegido. Cada consulta puede consumir créditos.
    </p>

    <div class="status-bar" id="statusBar">
        <span id="statusIcon">●</span>
        <span id="statusText">Ingresa The Odds API Key y presiona «Analizar Hoy»</span>
    </div>

    <div class="tabs">
        <button class="tab-btn active" data-tab="tab-fija">⭐ Fija del Día</button>
        <button class="tab-btn" data-tab="tab-top5">🏆 Top 5 Goles y Córners</button>
        <button class="tab-btn" data-tab="tab-todos">📋 Todos los Partidos</button>
    </div>

    <div class="tab-content active" id="tab-fija">
        <div id="fijaContainer">
            <div class="empty-state">
                <div class="empty-icon">📊</div>
                <h3>Esperando análisis</h3>
            </div>
        </div>
    </div>

    <div class="tab-content" id="tab-top5">
        <h3>📈 Selecciones con cuotas reales: goles y córners
            <span id="top5Count" style="color:#8ba0b8;font-size:13px">0 partidos</span>
        </h3>
        <div class="matches-grid" id="top5Grid"></div>
    </div>

    <div class="tab-content" id="tab-todos">
        <h3>📅 Todos los partidos de hoy
            <span id="todosCount" style="color:#8ba0b8;font-size:13px">0 partidos</span>
        </h3>
        <div class="matches-grid" id="todosGrid"></div>
    </div>
</div>

<script>
(() => {
    'use strict';

    const ODDS = 'https://api.the-odds-api.com/v4';
    const FOOTBALL = 'https://v3.football.api-sports.io';
    const ZONE = 'America/Lima';

    const $ = id => document.getElementById(id);

    const safe = value => String(value ?? '').replace(
        /[&<>"']/g,
        c => ({
            '&': '&amp;',
            '<': '&lt;',
            '>': '&gt;',
            '"': '&quot;',
            "'": '&#39;'
        })[c]
    );

    function dateStr(date) {
        const p = new Intl.DateTimeFormat('en-US', {
            timeZone: ZONE,
            year: 'numeric',
            month: '2-digit',
            day: '2-digit'
        }).formatToParts(date).reduce((obj, part) => {
            obj[part.type] = part.value;
            return obj;
        }, {});

        return `${p.year}-${p.month}-${p.day}`;
    }

    function clock(date) {
        return new Intl.DateTimeFormat('es-PE', {
            timeZone: ZONE,
            hour: '2-digit',
            minute: '2-digit'
        }).format(new Date(date));
    }

    function median(values) {
        const sorted = [...values].sort((a, b) => a - b);
        const i = Math.floor(sorted.length / 2);
        return sorted.length % 2
            ? sorted[i]
            : (sorted[i - 1] + sorted[i]) / 2;
    }

    function fmt(value) {
        return Number.isFinite(value) ? value.toFixed(2) : 'Sin datos';
    }

    function normalize(name) {
        return String(name || '')
            .toLowerCase()
            .normalize('NFD')
            .replace(/[\u0300-\u036f]/g, '')
            .replace(/\b(fc|cf|sc|ac|club|de|the)\b/g, '')
            .replace(/[^a-z0-9]/g, '');
    }

    const finished = new Set(['FT', 'AET', 'PEN']);

    let matches = [];
    let picks = [];
    let quota = '—';
    let running = false;
    let errors = [];

    function status(message, bad = false) {
        $('statusText').textContent = message;
        $('statusIcon').textContent = bad ? '✖' : '●';
        $('statusBar').classList.toggle('error', bad);
    }

    function empty(message) {
        return `
            <div class="empty-state">
                <div class="empty-icon">📊</div>
                <h3>${safe(message)}</h3>
            </div>
        `;
    }

    async function oddsGet(path, key, params = {}) {
        const url = new URL(ODDS + path);
        url.searchParams.set('apiKey', key);

        for (const [name, value] of Object.entries(params)) {
            url.searchParams.set(name, String(value));
        }

        let response;
        try {
            response = await fetch(url, { cache: 'no-store' });
        } catch {
            throw new Error('No se pudo conectar con The Odds API');
        }

        quota = response.headers.get('x-requests-remaining') || quota;
        const data = await response.json().catch(() => null);

        if (!response.ok) {
            throw new Error(
                `The Odds API ${response.status}: ${
                    data?.message || data?.error_code || 'respuesta no disponible'
                }`
            );
        }

        return data;
    }

    async function footballGet(path, key, params = {}) {
        const url = new URL(FOOTBALL + path);

        for (const [name, value] of Object.entries(params)) {
            url.searchParams.set(name, String(value));
        }

        let response;
        let data;

        try {
            response = await fetch(url, {
                headers: { 'x-apisports-key': key },
                cache: 'no-store'
            });
            data = await response.json().catch(() => null);

            if (response.status === 401 || response.status === 403) {
                response = await fetch(url, {
                    headers: {
                        'x-rapidapi-key': key,
                        'x-rapidapi-host': 'v3.football.api-sports.io'
                    },
                    cache: 'no-store'
                });
                data = await response.json().catch(() => null);
            }
        } catch {
            throw new Error('No se pudo conectar con API-Football');
        }

        if (
            !response.ok ||
            (data?.errors && Object.keys(data.errors).length)
        ) {
            throw new Error(
                `API-Football ${response.status}: ${JSON.stringify(data?.errors || {})}`
            );
        }

        return data?.response || [];
    }

    async function pool(items, concurrency, work) {
        let index = 0;

        await Promise.all(
            Array.from(
                { length: Math.min(concurrency, items.length) },
                async () => {
                    while (index < items.length) {
                        const current = index++;
                        await work(items[current], current);
                    }
                }
            )
        );
    }

    function marketsOf(event) {
        const groups = new Map();

        for (const bookmaker of event.bookmakers || []) {
            for (const market of bookmaker.markets || []) {
                if (
                    !['totals', 'alternate_totals_corners']
                        .includes(market.key)
                ) continue;

                const lines = new Map();

                for (const outcome of market.outcomes || []) {
                    const side = String(outcome.name || '').toLowerCase();
                    const line = Number(outcome.point);
                    const price = Number(outcome.price);

                    if (
                        !['over', 'under'].includes(side) ||
                        !Number.isFinite(line) ||
                        line % 1 !== 0.5 ||
                        !Number.isFinite(price) ||
                        price <= 1
                    ) continue;

                    if (!lines.has(line)) lines.set(line, {});

                    lines.get(line)[side] = {
                        price,
                        book: bookmaker.title || bookmaker.key,
                        updated: market.last_update ||
                                 bookmaker.last_update
                    };
                }

                for (const [line, row] of lines) {
                    if (!row.over || !row.under) continue;

                    const over = 1 / row.over.price;
                    const under = 1 / row.under.price;
                    const margin = over + under;

                    if (margin < 0.95 || margin > 1.4) continue;

                    for (const side of ['over', 'under']) {
                        const key = `${market.key}|${line}|${side}`;

                        if (!groups.has(key)) {
                            groups.set(key, {
                                kind: market.key === 'totals'
                                    ? 'Goles'
                                    : 'Córners',
                                side,
                                line,
                                quotes: []
                            });
                        }

                        groups.get(key).quotes.push({
                            prob: (side === 'over' ? over : under) / margin,
                            ...row[side]
                        });
                    }
                }
            }
        }

        return [...groups.values()]
            .map(group => {
                const probability = median(
                    group.quotes.map(quote => quote.prob)
                );

                const best = [...group.quotes]
                    .sort((a, b) => b.price - a.price)[0];

                return {
                    ...group,
                    prob: probability,
                    best,
                    books: group.quotes.length,
                    event,
                    score: probability * 100 +
                           Math.min(group.quotes.length, 5) * 0.7
                };
            })
            .filter(pick =>
                pick.prob >= 0.55 &&
                pick.best.price >= 1.4 &&
                pick.best.price <= 3
            );
    }

    function rank(a, b) {
        return b.score - a.score ||
               b.books - a.books ||
               Date.parse(a.event.commence_time) -
               Date.parse(b.event.commence_time);
    }

    function choose(list) {
        const sorted = [...list].sort(rank);
        const selected = [];
        const used = new Set();

        function add(kind, count) {
            for (const pick of sorted) {
                if (count === 0 || selected.length === 5) break;

                if (
                    pick.kind === kind &&
                    !used.has(pick.event.id)
                ) {
                    selected.push(pick);
                    used.add(pick.event.id);
                    count--;
                }
            }
        }

        if (
            sorted.some(p => p.kind === 'Goles') &&
            sorted.some(p => p.kind === 'Córners')
        ) {
            add('Goles', 3);
            add('Córners', 2);
        }

        for (const pick of sorted) {
            if (selected.length === 5) break;

            if (!used.has(pick.event.id)) {
                selected.push(pick);
                used.add(pick.event.id);
            }
        }

        return selected.sort(rank);
    }

    function matchFixture(event, fixtures) {
        const home = normalize(event.home_team);
        const away = normalize(event.away_team);
        const start = Date.parse(event.commence_time);

        const found = fixtures.filter(fixture =>
            normalize(fixture.teams?.home?.name) === home &&
            normalize(fixture.teams?.away?.name) === away &&
            Math.abs(Date.parse(fixture.fixture?.date) - start) <
                90 * 60000
        );

        return found.length === 1 ? found[0] : null;
    }

    function cornerNumber(team) {
        const value = (team?.statistics || [])
            .find(stat =>
                String(stat.type).toLowerCase() === 'corner kicks'
            )?.value;

        if (value === null || value === undefined) return null;

        const number = Number(value);
        return Number.isFinite(number) ? number : null;
    }

    async function history(teamId, key, cache) {
        if (cache.has(teamId)) return cache.get(teamId);

        const raw = await footballGet('/fixtures', key, {
            team: teamId,
            last: 8
        });

        const completed = raw.filter(fixture =>
            finished.has(fixture.fixture?.status?.short) &&
            Number.isFinite(fixture.goals?.home) &&
            Number.isFinite(fixture.goals?.away)
        );

        cache.set(teamId, completed);
        return completed;
    }

    async function enrich(
        pick,
        key,
        fixtures,
        teamCache,
        cornerCache
    ) {
        const fixture = matchFixture(pick.event, fixtures);
        if (!fixture) return;

        pick.fixture = fixture;

        const ids = [
            fixture.teams.home.id,
            fixture.teams.away.id
        ];

        const data = [];

        for (const id of ids) {
            let historicalMatches;

            try {
                historicalMatches = await history(
                    id,
                    key,
                    teamCache
                );
            } catch (error) {
                errors.push(error.message);
                continue;
            }

            let goalsFor = 0;
            let goalsAgainst = 0;
            let goalCount = 0;

            for (const game of historicalMatches) {
                const atHome = game.teams.home.id === id;

                if (
                    game.teams.away.id !== id &&
                    !atHome
                ) continue;

                goalsFor += atHome
                    ? game.goals.home
                    : game.goals.away;

                goalsAgainst += atHome
                    ? game.goals.away
                    : game.goals.home;

                goalCount++;
            }

            let cornersFor = 0;
            let cornersAgainst = 0;
            let cornerCount = 0;

            for (const game of historicalMatches.slice(0, 3)) {
                try {
                    let stats = cornerCache.get(game.fixture.id);

                    if (!stats) {
                        stats = await footballGet(
                            '/fixtures/statistics',
                            key,
                            { fixture: game.fixture.id }
                        );
                        cornerCache.set(game.fixture.id, stats);
                    }

                    const own = stats.find(
                        item => item.team?.id === id
                    );
                    const rival = stats.find(
                        item => item.team?.id !== id
                    );

                    const ownCorners = cornerNumber(own);
                    const rivalCorners = cornerNumber(rival);

                    if (
                        ownCorners !== null &&
                        rivalCorners !== null
                    ) {
                        cornersFor += ownCorners;
                        cornersAgainst += rivalCorners;
                        cornerCount++;
                    }
                } catch (error) {
                    errors.push(error.message);
                }
            }

            data.push({
                id,
                goalsN: goalCount,
                gf: goalCount ? goalsFor / goalCount : null,
                ga: goalCount ? goalsAgainst / goalCount : null,
                cornersN: cornerCount,
                cf: cornerCount ? cornersFor / cornerCount : null,
                ca: cornerCount ? cornersAgainst / cornerCount : null
            });
        }

        pick.history = data;
    }

    function recentLine(pick) {
        if (!pick.history?.length) {
            return 'Historial no disponible o partido sin correspondencia segura en API-Football.';
        }

        return pick.history.map((stat, index) => {
            const team = index === 0 ? 'Local' : 'Visitante';

            const goals = stat.goalsN
                ? `${fmt(stat.gf)} GF, ${fmt(stat.ga)} GC (${stat.goalsN} partidos)`
                : 'sin goles históricos';

            const corners = stat.cornersN
                ? `${fmt(stat.cf)} córners a favor, ${fmt(stat.ca)} en contra (${stat.cornersN} partidos)`
                : 'sin córners históricos';

            return `${team}: ${goals}; ${corners}`;
        }).join(' · ');
    }

    function stat(label, value) {
        return `
            <div class="stat">
                <span class="label">${label}</span>
                <span class="value">${value}</span>
            </div>
        `;
    }

    function card(pick, rankNumber = 0) {
        const event = pick.event;
        const top = rankNumber === 1;
        const direction = pick.side === 'over'
            ? 'Más'
            : 'Menos';

        const updated =
            pick.best.updated &&
            Number.isFinite(Date.parse(pick.best.updated))
                ? clock(pick.best.updated)
                : 'sin hora';

        return `
            <div class="match-card ${top ? 'top-match' : ''}">
                ${rankNumber
                    ? `<span class="badge-top">#${rankNumber}</span>`
                    : ''}

                <div class="match-header">
                    <span class="league">
                        ${safe(event.sport_title || event.sport_key)}
                    </span>
                    <span>🕐 ${safe(clock(event.commence_time))} Perú</span>
                    <span class="data-source-badge">✅ Cuota real</span>
                </div>

                <div class="teams">
                    <span>🏠 ${safe(event.home_team)}</span>
                    <span class="vs">vs</span>
                    <span>${safe(event.away_team)} ✈️</span>
                </div>

                <div class="tipster-stats">
                    ${stat('🎯 Mercado', safe(pick.kind))}
                    ${stat('📊 Consenso sin margen',
                        (pick.prob * 100).toFixed(1) + ' %')}
                    ${stat('💰 Mejor cuota',
                        pick.best.price.toFixed(2))}
                    ${stat('🏦 Casas', String(pick.books))}
                    ${stat('🕐 Actualizada', safe(updated))}
                    ${stat('📅 Historial',
                        pick.history?.length
                            ? 'Disponible'
                            : 'Sin datos')}
                </div>

                <div class="prediction-line">
                    <span>
                        ⚡ <span class="favorite">
                            ${direction} de ${pick.line.toFixed(1)}
                            ${pick.kind.toLowerCase()}
                        </span>
                    </span>
                    <span class="over-under">
                        ${safe(pick.best.book)}
                    </span>
                </div>

                <p style="color:#8ba0b8;font-size:12px;line-height:1.5">
                    ${safe(recentLine(pick))}
                </p>
            </div>
        `;
    }

    function fixtureCard(event) {
        return `
            <div class="match-card">
                <div class="match-header">
                    <span class="league">
                        ${safe(event.sport_title || event.sport_key)}
                    </span>
                    <span>🕐 ${safe(clock(event.commence_time))} Perú</span>
                </div>

                <div class="teams">
                    <span>🏠 ${safe(event.home_team)}</span>
                    <span class="vs">vs</span>
                    <span>${safe(event.away_team)} ✈️</span>
                </div>

                <div class="prediction-line">
                    ${event.queried
                        ? 'Sin línea elegible'
                        : 'No consultado (límite de análisis)'}
                </div>
            </div>
        `;
    }

    function render() {
        const top = choose(picks);

        $('top5Grid').innerHTML = top.length
            ? top.map((pick, index) =>
                card(pick, index + 1)
              ).join('')
            : empty(
                'No hay apuestas verificables con los filtros actuales'
              );

        $('top5Count').textContent =
            `${top.length} apuestas`;

        const bestByEvent = new Map();

        for (const pick of [...picks].sort(rank)) {
            if (!bestByEvent.has(pick.event.id)) {
                bestByEvent.set(pick.event.id, pick);
            }
        }

        $('todosGrid').innerHTML = matches.length
            ? matches.map(event =>
                bestByEvent.has(event.id)
                    ? card(bestByEvent.get(event.id))
                    : fixtureCard(event)
              ).join('')
            : empty('No se encontraron partidos');

        $('todosCount').textContent =
            `${matches.length} partidos`;

        const first = top[0];

        $('fijaContainer').innerHTML = first
            ? `
                <div class="fija-section">
                    <div class="fija-header">
                        <span>⭐</span>
                        <h2>Selección destacada</h2>
                        <span class="fija-badge">
                            ${safe(first.kind)}
                        </span>
                    </div>

                    <div class="fija-match">
                        <span class="team-name local">
                            ${safe(first.event.home_team)}
                        </span>
                        <span class="vs-text">vs</span>
                        <span class="team-name">
                            ${safe(first.event.away_team)}
                        </span>
                        <span class="prediction">
                            ${first.side === 'over'
                                ? 'Más'
                                : 'Menos'}
                            de ${first.line.toFixed(1)}
                            ${first.kind.toLowerCase()}
                        </span>
                    </div>

                    <div class="fija-stats-grid">
                        <div class="fs-item">
                            <span class="fs-label">
                                Cuota · ${safe(first.best.book)}
                            </span>
                            <span class="fs-value">
                                ${first.best.price.toFixed(2)}
                            </span>
                        </div>

                        <div class="fs-item">
                            <span class="fs-label">
                                Consenso sin margen
                            </span>
                            <span class="fs-value">
                                ${(first.prob * 100).toFixed(1)} %
                            </span>
                        </div>

                        <div class="fs-item">
                            <span class="fs-label">
                                Casas con ambos lados
                            </span>
                            <span class="fs-value">
                                ${first.books}
                            </span>
                        </div>
                    </div>

                    <div class="fija-args">
                        <div class="arg-item">
                            <div class="arg-num">
                                Datos del mercado
                            </div>
                            <div class="arg-text">
                                Cuota publicada para esta línea.
                                El consenso se calculó con las
                                cuotas Over y Under de la misma línea
                                en ${first.books}
                                ${first.books === 1
                                    ? 'casa'
                                    : 'casas'}.
                                No es una predicción independiente.
                            </div>
                        </div>

                        <div class="arg-item">
                            <div class="arg-num">
                                Historial real disponible
                            </div>
                            <div class="arg-text">
                                ${safe(recentLine(first))}
                            </div>
                        </div>
                    </div>
                </div>
              `
            : empty(
                'No hay selección destacada verificable'
              );

        $('statusDot').className = 'dot pending';
    }

    async function analyze() {
        if (running) return;

        const oddsKey = $('apiKeyOdds').value.trim();
        const footballKey = $('apiKeyFootball').value.trim();

        if (!oddsKey) {
            status(
                'Ingresa la clave de The Odds API para consultar cuotas reales.',
                true
            );
            return;
        }

        running = true;
        $('btnAnalizar').disabled = true;
        $('btnActualizar').disabled = true;

        errors = [];
        quota = '—';
        matches = [];
        picks = [];
        render();

        try {
            status('Buscando partidos de hoy en ligas activas...');

            const sports = (await oddsGet('/sports', oddsKey))
                .filter(sport =>
                    sport.active &&
                    sport.key?.startsWith('soccer_') &&
                    !sport.has_outrights
                );

            if (!sports.length) {
                throw new Error(
                    'No hay ligas activas en la API'
                );
            }

            const allEvents = [];

            await pool(sports, 5, async sport => {
                try {
                    const list = await oddsGet(
                        `/sports/${encodeURIComponent(sport.key)}/events`,
                        oddsKey
                    );

                    for (const event of Array.isArray(list)
                        ? list
                        : []) {
                        if (
                            event.id &&
                            Date.parse(event.commence_time) >
                                Date.now() &&
                            dateStr(new Date(event.commence_time)) ===
                                dateStr(new Date())
                        ) {
                            allEvents.push({
                                ...event,
                                sport_title:
                                    event.sport_title ||
                                    sport.title
                            });
                        }
                    }
                } catch (error) {
                    errors.push(
                        `${sport.key}: ${error.message}`
                    );
                }
            });

            matches = [...new Map(
                allEvents.map(event => [
                    event.id,
                    event
                ])
            ).values()].sort(
                (a, b) =>
                    Date.parse(a.commence_time) -
                    Date.parse(b.commence_time)
            );

            if (!matches.length) {
                render();

                status(
                    errors.length === sports.length
                        ? errors[0]
                        : 'No hay partidos por comenzar hoy (hora de Perú).',
                    errors.length === sports.length
                );

                return;
            }

            const limit = Number($('limit').value) || 40;
            const region = $('region').value;
            const subset = matches.slice(0, limit);

            status(
                `${matches.length} partidos encontrados; consultando ${subset.length} mercados de goles y córners...`
            );

            let completed = 0;

            await pool(subset, 3, async event => {
                try {
                    const data = await oddsGet(
                        `/sports/${encodeURIComponent(
                            event.sport_key
                        )}/events/${encodeURIComponent(
                            event.id
                        )}/odds`,
                        oddsKey,
                        {
                            regions: region,
                            markets:
                                'totals,alternate_totals_corners',
                            oddsFormat: 'decimal'
                        }
                    );

                    event.queried = true;

                    if (data?.bookmakers) {
                        picks.push(
                            ...marketsOf({
                                ...event,
                                bookmakers: data.bookmakers
                            })
                        );
                    }
                } catch (error) {
                    errors.push(
                        `${event.home_team}: ${error.message}`
                    );
                } finally {
                    completed++;

                    if (
                        completed % 5 === 0 ||
                        completed === subset.length
                    ) {
                        status(
                            `Mercados consultados: ${completed}/${subset.length}; ${errors.length} errores.`
                        );
                    }
                }
            });

            if (footballKey && picks.length) {
                try {
                    status(
                        'Consultando historial real de goles y córners...'
                    );

                    const fixtures = await footballGet(
                        '/fixtures',
                        footballKey,
                        {
                            date: dateStr(new Date()),
                            timezone: ZONE
                        }
                    );

                    const preliminary = choose(picks);
                    const teamCache = new Map();
                    const cornerCache = new Map();

                    for (const pick of preliminary) {
                        await enrich(
                            pick,
                            footballKey,
                            fixtures,
                            teamCache,
                            cornerCache
                        );
                    }
                } catch (error) {
                    errors.push(error.message);
                }
            }

            render();

            status(
                `Análisis completo: ${matches.length} partidos, ${subset.length} mercados consultados, ${choose(picks).length} selecciones. Créditos Odds restantes: ${quota}.` +
                (errors.length
                    ? ` Errores: ${errors.length} (primero: ${errors[0]}).`
                    : ''),
                errors.length > 0 && picks.length === 0
            );
        } catch (error) {
            render();
            status(error.message, true);
        } finally {
            running = false;
            $('btnAnalizar').disabled = false;
            $('btnActualizar').disabled = false;
        }
    }

    $('fechaTexto').textContent =
        new Intl.DateTimeFormat('es-PE', {
            timeZone: ZONE,
            weekday: 'long',
            day: 'numeric',
            month: 'long',
            year: 'numeric'
        }).format(new Date()) + ' · Perú';

    $('btnAnalizar').addEventListener(
        'click',
        analyze
    );

    $('btnActualizar').addEventListener(
        'click',
        analyze
    );

    $('btnGuardarKeys').addEventListener(
        'click',
        () => {
            const football =
                $('apiKeyFootball').value.trim();

            const odds =
                $('apiKeyOdds').value.trim();

            if (!football && !odds) {
                status(
                    'Ingresa al menos una clave.',
                    true
                );
                return;
            }

            if (football) {
                sessionStorage.setItem(
                    'pro_tipster_football_key',
                    football
                );
            }

            if (odds) {
                sessionStorage.setItem(
                    'pro_tipster_odds_key',
                    odds
                );
            }

            status(
                'Claves guardadas para esta pestaña; no están escritas en el archivo.'
            );
        }
    );

    $('apiKeyFootball').value =
        sessionStorage.getItem(
            'pro_tipster_football_key'
        ) || '';

    $('apiKeyOdds').value =
        sessionStorage.getItem(
            'pro_tipster_odds_key'
        ) || '';

    document.querySelectorAll('.tab-btn')
        .forEach(button => {
            button.addEventListener(
                'click',
                () => {
                    document
                        .querySelectorAll(
                            '.tab-btn,.tab-content'
                        )
                        .forEach(item =>
                            item.classList.remove('active')
                        );

                    button.classList.add('active');
                    $(button.dataset.tab)
                        .classList.add('active');
                }
            );
        });
})();
</script>
</body>
</html>
