[krypto_ai_trading_simulator.html](https://github.com/user-attachments/files/34315961/krypto_ai_trading_simulator.html)
<!DOCTYPE html>
<html lang="sv" class="dark">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Krypto AI Trading Simulator</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Lucide Icons -->
    <script src="https://unpkg.com/lucide@latest"></script>
    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: {
                extend: {
                    colors: {
                        trade: {
                            dark: '#0b0e11',
                            panel: '#181a20',
                            border: '#2b2f36',
                            hover: '#262930',
                            green: '#0ecb81',
                            greenHover: '#0bb371',
                            red: '#f6465d',
                            redHover: '#d93d52',
                            textMuted: '#848e9c'
                        }
                    }
                }
            }
        }
    </script>
    <style>
        /* Custom scrollbar for trading terminal aesthetic */
        ::-webkit-scrollbar {
            width: 5px;
            height: 5px;
        }
        ::-webkit-scrollbar-track {
            background: #181a20;
        }
        ::-webkit-scrollbar-thumb {
            background: #2b2f36;
            border-radius: 3px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #474d57;
        }
        body {
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
            user-select: none;
        }
    </style>
</head>
<body class="bg-trade-dark text-gray-200 h-screen flex flex-col overflow-hidden">

    <!-- Top Navigation Header -->
    <header class="h-14 bg-trade-panel border-b border-trade-border flex items-center justify-between px-4 shrink-0">
        <div class="flex items-center space-x-6">
            <div class="flex items-center space-x-2">
                <div class="bg-yellow-500 text-black p-1.5 rounded-lg font-bold text-xl flex items-center justify-center">
                    <i data-lucide="bot" class="w-5 h-5"></i>
                </div>
                <span class="font-extrabold text-lg tracking-wide text-white">NEXUS<span class="text-yellow-500">AI</span> TRADER</span>
            </div>
            
            <!-- Asset Selector -->
            <div class="flex items-center space-x-4 border-l border-trade-border pl-4">
                <select id="assetSelect" class="bg-trade-dark border border-trade-border text-white text-sm rounded px-3 py-1.5 focus:outline-none focus:border-yellow-500 cursor-pointer font-semibold">
                    <option value="BTCUSDT">BTC / USDT - Bitcoin</option>
                    <option value="ETHUSDT">ETH / USDT - Ethereum</option>
                    <option value="SOLUSDT">SOL / USDT - Solana</option>
                </select>
                <div class="flex flex-col">
                    <span id="headerPrice" class="text-base font-bold text-trade-green">675 000,00 kr</span>
                    <span id="headerChange" class="text-xs text-trade-green font-medium">+2.45%</span>
                </div>
            </div>
        </div>

        <!-- Right Header Stats & Actions -->
        <div class="flex items-center space-x-4">
            <!-- Currency Selector -->
            <div class="flex items-center space-x-1">
                <i data-lucide="coins" class="w-4 h-4 text-trade-textMuted"></i>
                <select id="currencySelect" class="bg-trade-dark border border-trade-border text-white text-xs rounded px-2 py-1 focus:outline-none focus:border-yellow-500 cursor-pointer">
                    <option value="SEK">SEK (kr)</option>
                    <option value="USD">USD ($)</option>
                    <option value="EUR">EUR (€)</option>
                    <option value="GBP">GBP (£)</option>
                </select>
            </div>

            <!-- Language Selector -->
            <div class="flex items-center space-x-1">
                <i data-lucide="globe" class="w-4 h-4 text-trade-textMuted"></i>
                <select id="langSelect" class="bg-trade-dark border border-trade-border text-white text-xs rounded px-2 py-1 focus:outline-none focus:border-yellow-500 cursor-pointer">
                    <option value="sv">SV 🇸🇪</option>
                    <option value="en">EN 🇬🇧</option>
                </select>
            </div>

            <!-- AI Trading Switch -->
            <div class="flex items-center space-x-3 bg-trade-dark px-3 py-1.5 rounded-lg border border-trade-border">
                <div class="flex items-center space-x-2">
                    <i data-lucide="cpu" class="w-4 h-4 text-yellow-500 animate-pulse"></i>
                    <span class="text-xs font-semibold uppercase tracking-wider text-gray-300">AI Auto-Bot</span>
                </div>
                <label class="relative inline-flex items-center cursor-pointer">
                    <input type="checkbox" id="aiBotToggle" class="sr-only peer">
                    <div class="w-9 h-5 bg-gray-700 peer-focus:outline-none rounded-full peer peer-checked:after:translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-[2px] after:left-[2px] after:bg-white after:border-gray-300 after:border after:rounded-full after:h-4 after:w-4 after:transition-all peer-checked:bg-yellow-500"></div>
                </label>
            </div>

            <!-- Total Balance Display -->
            <div class="flex flex-col text-right">
                <span id="lblTotalBalance" class="text-xs text-trade-textMuted">Totalt Portföljvärde</span>
                <span id="headerTotalBalance" class="text-sm font-bold text-white">1 000,00 kr</span>
            </div>

            <!-- Add Funds & Reset Buttons -->
            <div class="flex items-center space-x-2">
                <button id="addFundsBtn" class="flex items-center space-x-1 bg-trade-green hover:bg-trade-greenHover text-white text-xs px-3 py-1.5 rounded font-bold transition shadow" title="Sätt in 10 000 kr">
                    <i data-lucide="plus-circle" class="w-3.5 h-3.5"></i>
                    <span id="lblAddFundsBtn">+10 000 kr</span>
                </button>

                <button id="resetBtn" class="flex items-center space-x-1 bg-trade-border hover:bg-gray-700 text-xs px-3 py-1.5 rounded text-gray-300 transition" title="Återställ konto">
                    <i data-lucide="rotate-ccw" class="w-3.5 h-3.5"></i>
                    <span id="lblResetBtn">Återställ</span>
                </button>
            </div>
        </div>
    </header>

    <!-- Main Workspace -->
    <div class="flex-1 flex overflow-hidden">
        
        <!-- Left Sidebar: Market & AI Signals -->
        <aside class="w-72 bg-trade-panel border-r border-trade-border flex flex-col shrink-0">
            <!-- AI Signal Box -->
            <div class="p-3 border-b border-trade-border bg-gradient-to-b from-trade-panel to-trade-dark">
                <div class="flex items-center justify-between mb-2">
                    <span id="lblAiAnalysis" class="text-xs font-bold text-yellow-500 uppercase tracking-wider flex items-center gap-1.5">
                        <i data-lucide="sparkles" class="w-4 h-4"></i> AI Marknadsanalys
                    </span>
                    <span id="aiConfidence" class="text-xs bg-yellow-500/10 text-yellow-500 border border-yellow-500/20 px-1.5 py-0.5 rounded font-mono">87% Säkerhet</span>
                </div>
                <div id="aiSignalBox" class="p-3 rounded-lg bg-trade-dark border border-trade-border flex flex-col items-center justify-center text-center">
                    <span id="aiSignalText" class="text-lg font-black text-trade-green tracking-wide">STARK KÖP (BUY)</span>
                    <p id="aiSignalReason" class="text-xs text-trade-textMuted mt-1">RSI oversold (28.4), MACD bullish crossover bekräftad på 5m.</p>
                </div>
            </div>

            <!-- Indicator Stats -->
            <div class="p-3 border-b border-trade-border grid grid-cols-3 gap-2 text-center text-xs">
                <div class="bg-trade-dark p-2 rounded border border-trade-border">
                    <div class="text-trade-textMuted">RSI (14)</div>
                    <div id="rsiValue" class="font-bold text-trade-green mt-0.5">32.1</div>
                </div>
                <div class="bg-trade-dark p-2 rounded border border-trade-border">
                    <div class="text-trade-textMuted">MACD</div>
                    <div id="macdValue" class="font-bold text-trade-green mt-0.5">+14.2</div>
                </div>
                <div class="bg-trade-dark p-2 rounded border border-trade-border">
                    <div id="lblTrend" class="text-trade-textMuted">Trend (MA)</div>
                    <div id="maTrend" class="font-bold text-trade-green mt-0.5">Bullish</div>
                </div>
            </div>

            <!-- Live Order Book Simulation -->
            <div class="flex-1 flex flex-col overflow-hidden p-3">
                <span id="lblOrderBook" class="text-xs font-bold text-trade-textMuted uppercase tracking-wider mb-2">Orderbok (Live)</span>
                
                <!-- Ask Orders (Red) -->
                <div id="orderBookAsks" class="flex flex-col-reverse text-xs space-y-0.5 space-y-reverse overflow-hidden font-mono">
                    <!-- Dynamic asks -->
                </div>

                <!-- Spread / Current Price -->
                <div class="py-2 border-y border-trade-border my-1 flex justify-between items-center px-1 font-mono">
                    <span id="obCurrentPrice" class="text-sm font-bold text-trade-green">675 000,00 kr</span>
                    <span id="lblSpread" class="text-[10px] text-trade-textMuted">Spridning: 0.01%</span>
                </div>

                <!-- Bid Orders (Green) -->
                <div id="orderBookBids" class="flex flex-col text-xs space-y-0.5 overflow-hidden font-mono">
                    <!-- Dynamic bids -->
                </div>
            </div>
        </aside>

        <!-- Middle Section: Candlestick Chart & Tabs -->
        <main class="flex-1 flex flex-col bg-trade-dark overflow-hidden">
            
            <!-- Chart Toolbar -->
            <div class="h-10 border-b border-trade-border flex items-center justify-between px-4 bg-trade-panel shrink-0">
                <div class="flex items-center space-x-2">
                    <span id="lblTimeframe" class="text-xs font-medium text-trade-textMuted mr-2">Tidsram:</span>
                    <button class="tf-btn text-xs px-2.5 py-1 rounded bg-trade-dark border border-yellow-500 text-yellow-500 font-semibold" data-tf="1m">1m</button>
                    <button class="tf-btn text-xs px-2.5 py-1 rounded bg-trade-dark text-trade-textMuted hover:text-white" data-tf="5m">5m</button>
                    <button class="tf-btn text-xs px-2.5 py-1 rounded bg-trade-dark text-trade-textMuted hover:text-white" data-tf="15m">15m</button>
                    <button class="tf-btn text-xs px-2.5 py-1 rounded bg-trade-dark text-trade-textMuted hover:text-white" data-tf="1h">1h</button>
                </div>

                <div class="flex items-center space-x-4 text-xs font-mono text-trade-textMuted">
                    <span>O: <strong id="ohlcO" class="text-gray-200">0 kr</strong></span>
                    <span>H: <strong id="ohlcH" class="text-gray-200">0 kr</strong></span>
                    <span>L: <strong id="ohlcL" class="text-gray-200">0 kr</strong></span>
                    <span>C: <strong id="ohlcC" class="text-gray-200">0 kr</strong></span>
                </div>
            </div>

            <!-- Canvas Container -->
            <div class="flex-1 relative w-full h-full min-h-[300px]">
                <canvas id="candleCanvas" class="absolute inset-0 w-full h-full cursor-crosshair"></canvas>
            </div>

            <!-- Bottom Panel: Positions, Pending Orders, Trade History -->
            <div class="h-56 border-t border-trade-border bg-trade-panel flex flex-col shrink-0">
                <!-- Tabs header -->
                <div class="flex border-b border-trade-border px-3 text-xs font-semibold text-trade-textMuted">
                    <button id="tabPositionsBtn" class="py-2.5 px-4 text-yellow-500 border-b-2 border-yellow-500 font-bold focus:outline-none"><span id="lblOpenPositions">Öppna Positioner</span> (<span id="posCount">0</span>)</button>
                    <button id="tabHistoryBtn" class="py-2.5 px-4 hover:text-white focus:outline-none"><span id="lblOrderHistory">Orderhistorik</span></button>
                    <button id="tabLogsBtn" class="py-2.5 px-4 hover:text-white focus:outline-none"><span id="lblAiLogs">AI Loggar</span></button>
                </div>

                <!-- Tab Content Area -->
                <div class="flex-1 overflow-y-auto p-2">
                    
                    <!-- Table: Open Positions -->
                    <div id="tabPositions" class="w-full">
                        <table class="w-full text-left text-xs text-gray-300 font-mono">
                            <thead class="text-trade-textMuted border-b border-trade-border bg-trade-dark/40 sticky top-0">
                                <tr>
                                    <th class="p-2" id="thAsset">Tillgång</th>
                                    <th class="p-2" id="thType">Typ</th>
                                    <th class="p-2" id="thVolume">Volym</th>
                                    <th class="p-2" id="thEntry">Instegspris</th>
                                    <th class="p-2" id="thMarket">Marknadspris</th>
                                    <th class="p-2" id="thLeverage">Hävstång</th>
                                    <th class="p-2" id="thPnl">PnL (Vinst/Förlust)</th>
                                    <th class="p-2 text-right" id="thAction">Åtgärd (Sälj / Stäng)</th>
                                </tr>
                            </thead>
                            <tbody id="positionsTableBody">
                                <!-- Dynamic positions rows -->
                            </tbody>
                        </table>
                    </div>

                    <!-- Table: Order History -->
                    <div id="tabHistory" class="w-full hidden">
                        <table class="w-full text-left text-xs text-gray-300 font-mono">
                            <thead class="text-trade-textMuted border-b border-trade-border bg-trade-dark/40 sticky top-0">
                                <tr>
                                    <th class="p-2" id="thTime">Tidpunkt</th>
                                    <th class="p-2" id="thHAsset">Tillgång</th>
                                    <th class="p-2" id="thHType">Typ</th>
                                    <th class="p-2" id="thHEntry">Instegspris</th>
                                    <th class="p-2" id="thHExit">Stängningspris</th>
                                    <th class="p-2" id="thHNet">Netto Resultat</th>
                                </tr>
                            </thead>
                            <tbody id="historyTableBody">
                                <!-- Dynamic history rows -->
                            </tbody>
                        </table>
                    </div>

                    <!-- AI Logs Console -->
                    <div id="tabLogs" class="w-full hidden font-mono text-xs text-gray-400 p-2 space-y-1">
                        <div class="text-yellow-500">[SYSTEM] AI Trading Engine initialized. Monitoring market liquidity...</div>
                    </div>

                </div>
            </div>
        </main>

        <!-- Right Panel: Order Form & Portfolio Balances -->
        <aside class="w-80 bg-trade-panel border-l border-trade-border flex flex-col p-4 shrink-0 overflow-y-auto">
            
            <!-- Trading Mode Switch -->
            <div class="mb-3">
                <div class="flex justify-between items-center text-xs text-trade-textMuted mb-1.5 font-semibold">
                    <span id="lblTradingMode">Handelsläge</span>
                </div>
                <div class="grid grid-cols-2 gap-1 bg-trade-dark p-1 rounded-lg border border-trade-border text-xs">
                    <button id="modeMarginBtn" class="py-1.5 rounded font-bold bg-trade-border text-white">Margin</button>
                    <button id="modeSpotBtn" class="py-1.5 rounded font-bold text-trade-textMuted hover:text-white">Spot</button>
                </div>
            </div>

            <!-- Trading Tabs: Buy / Sell -->
            <div class="grid grid-cols-2 gap-2 mb-3 bg-trade-dark p-1 rounded-lg border border-trade-border">
                <button id="orderTypeBuy" class="py-2 text-xs font-bold rounded-md bg-trade-green text-white shadow-lg transition">KÖP (LONG)</button>
                <button id="orderTypeSell" class="py-2 text-xs font-bold rounded-md text-trade-textMuted hover:text-white transition">SÄLJ (SHORT / SPOT)</button>
            </div>

            <!-- Leverage Slider -->
            <div id="leverageGroup" class="mb-4 bg-trade-dark p-2.5 rounded-lg border border-trade-border">
                <div class="flex justify-between items-center mb-1">
                    <span id="lblLeverage" class="text-xs text-trade-textMuted">Hävstång (Leverage)</span>
                    <span id="leverageVal" class="text-xs font-bold text-yellow-500">1x</span>
                </div>
                <input type="range" id="leverageRange" min="1" max="20" value="1" class="w-full accent-yellow-500 cursor-pointer">
            </div>

            <!-- Amount Input -->
            <div class="mb-3">
                <div class="flex justify-between items-center mb-1">
                    <label id="amountInputLabel" class="text-[11px] text-trade-textMuted">Belopp</label>
                    <span class="text-[11px] text-trade-textMuted"><span id="lblAvailable">Tillgängligt</span>: <strong id="availUsdt" class="text-gray-200 font-mono">1 000,00 kr</strong></span>
                </div>
                <div class="relative">
                    <input type="number" id="tradeAmountInput" class="w-full bg-trade-dark border border-trade-border text-white text-sm rounded px-3 py-2 font-mono focus:outline-none focus:border-yellow-500" placeholder="100">
                    <span id="amountUnitLabel" class="absolute right-3 top-2.5 text-xs text-trade-textMuted">kr</span>
                </div>
            </div>

            <!-- Quick Percentage Buttons -->
            <div class="grid grid-cols-4 gap-1.5 mb-5">
                <button class="pct-btn bg-trade-dark hover:bg-trade-border text-xs py-1.5 rounded font-mono text-gray-300" data-pct="25">25%</button>
                <button class="pct-btn bg-trade-dark hover:bg-trade-border text-xs py-1.5 rounded font-mono text-gray-300" data-pct="50">50%</button>
                <button class="pct-btn bg-trade-dark hover:bg-trade-border text-xs py-1.5 rounded font-mono text-gray-300" data-pct="75">75%</button>
                <button class="pct-btn bg-trade-dark hover:bg-trade-border text-xs py-1.5 rounded font-mono text-gray-300" data-pct="100">100%</button>
            </div>

            <!-- Execute Button -->
            <button id="executeTradeBtn" class="w-full bg-trade-green hover:bg-trade-greenHover text-white font-bold py-3 rounded-lg text-sm tracking-wide shadow-lg transition uppercase flex items-center justify-center space-x-2 mb-6">
                <i data-lucide="arrow-up-right" class="w-4 h-4"></i>
                <span id="executeBtnText">Öppna Köp / Long Position</span>
            </button>

            <!-- Portfolio Balances Card -->
            <div class="mt-auto bg-trade-dark p-3.5 rounded-lg border border-trade-border">
                <h4 class="text-xs font-bold text-gray-300 uppercase tracking-wider mb-2.5 flex items-center justify-between">
                    <span id="lblPortfolioHeading">Saldon & Portfölj</span>
                    <i data-lucide="wallet" class="w-3.5 h-3.5 text-trade-textMuted"></i>
                </h4>
                <div class="space-y-2 text-xs font-mono">
                    <div class="flex justify-between items-center border-b border-trade-border/50 pb-1.5">
                        <span id="lblCashBalance" class="text-trade-textMuted">Kontanter:</span>
                        <span id="walletUsdt" class="text-white font-semibold">1 000,00 kr</span>
                    </div>
                    <div class="flex justify-between items-center border-b border-trade-border/50 pb-1.5">
                        <span class="text-trade-textMuted">BTC:</span>
                        <span id="walletBtc" class="text-white font-semibold">0.0000 BTC</span>
                    </div>
                    <div class="flex justify-between items-center border-b border-trade-border/50 pb-1.5">
                        <span class="text-trade-textMuted">ETH:</span>
                        <span id="walletEth" class="text-white font-semibold">0.0000 ETH</span>
                    </div>
                    <div class="flex justify-between items-center">
                        <span class="text-trade-textMuted">SOL:</span>
                        <span id="walletSol" class="text-white font-semibold">0.0000 SOL</span>
                    </div>
                </div>
            </div>

        </aside>

    </div>

    <!-- Application Engine JavaScript -->
    <script>
        // Global Canvas Context Variables
        let canvas, ctx;

        // Base currency rate conversions from SEK
        const currencyRates = {
            SEK: { rate: 1, symbol: 'kr', pos: 'after', locale: 'sv-SE' },
            USD: { rate: 0.096, symbol: '$', pos: 'before', locale: 'en-US' },
            EUR: { rate: 0.088, symbol: '€', pos: 'before', locale: 'de-DE' },
            GBP: { rate: 0.075, symbol: '£', pos: 'before', locale: 'en-GB' }
        };

        // Timeframe Interval Configurations (in ms)
        const timeframeConfigs = {
            '1m': { intervalMs: 60 * 1000, candleCount: 80, volFactor: 0.025 },
            '5m': { intervalMs: 5 * 60 * 1000, candleCount: 80, volFactor: 0.045 },
            '15m': { intervalMs: 15 * 60 * 1000, candleCount: 80, volFactor: 0.080 },
            '1h': { intervalMs: 60 * 60 * 1000, candleCount: 80, volFactor: 0.150 }
        };

        // Storage Key
        const STORAGE_KEY = 'NEXUS_CRYPTO_SIM_STATE';

        // i18n Translations Dictionary
        const translations = {
            sv: {
                totalVal: 'Totalt Portföljvärde',
                addFundsBtn: '+10 000 kr',
                resetBtn: 'Återställ',
                aiAnalysis: 'AI Marknadsanalys',
                confidence: 'Säkerhet',
                trend: 'Trend (MA)',
                orderBook: 'Orderbok (Live)',
                spread: 'Spridning',
                timeframe: 'Tidsram:',
                openPos: 'Öppna Positioner',
                orderHist: 'Orderhistorik',
                aiLogs: 'AI Loggar',
                thAsset: 'Tillgång',
                thType: 'Typ',
                thVolume: 'Volym',
                thEntry: 'Instegspris',
                thMarket: 'Marknadspris',
                thLeverage: 'Hävstång',
                thPnl: 'PnL (Vinst/Förlust)',
                thAction: 'Åtgärd',
                thTime: 'Tidpunkt',
                thNet: 'Netto Resultat',
                tradeMode: 'Handelsläge',
                buyLong: 'KÖP (LONG)',
                sellShort: 'SÄLJ (SHORT / SPOT)',
                leverage: 'Hävstång (Leverage)',
                amount: 'Belopp',
                available: 'Tillgängligt',
                portfolio: 'Saldon & Portfölj',
                cash: 'Kontanter:',
                closeBtn: 'Stäng / Sälj',
                noPos: 'Inga öppna positioner för tillfället.',
                noHist: 'Ingen stängd orderhistorik än.',
                buySpotText: (name) => `KÖP ${name} (SPOT)`,
                sellSpotText: (name) => `SÄLJA ${name} (SPOT)`,
                buyLongText: 'Öppna Köp / Long Position',
                sellShortText: 'Öppna Sälj / Short Position',
                signalBuy: 'STARK KÖP (BUY)',
                signalSell: 'STARK SÄLJ (SELL)',
                signalNeutral: 'NEUTRAL / AVVAKTA',
                reasonBuy: (rsi) => `RSI indikerar översålt läge (${rsi}). Prisrekyl uppåt är sannolik.`,
                reasonSell: (rsi) => `RSI indikerar överköpt läge (${rsi}). Säljtryck förväntas.`,
                reasonNeutral: (rsi) => `RSI är i balans (${rsi}). Inga starka utbrottstendenser.`
            },
            en: {
                totalVal: 'Total Portfolio Value',
                addFundsBtn: '+10,000 kr',
                resetBtn: 'Reset',
                aiAnalysis: 'AI Market Analysis',
                confidence: 'Confidence',
                trend: 'Trend (MA)',
                orderBook: 'Order Book (Live)',
                spread: 'Spread',
                timeframe: 'Timeframe:',
                openPos: 'Open Positions',
                orderHist: 'Order History',
                aiLogs: 'AI Logs',
                thAsset: 'Asset',
                thType: 'Type',
                thVolume: 'Volume',
                thEntry: 'Entry Price',
                thMarket: 'Market Price',
                thLeverage: 'Leverage',
                thPnl: 'PnL (Profit/Loss)',
                thAction: 'Action',
                thTime: 'Timestamp',
                thNet: 'Net Result',
                tradeMode: 'Trade Mode',
                buyLong: 'BUY (LONG)',
                sellShort: 'SELL (SHORT / SPOT)',
                leverage: 'Leverage',
                amount: 'Amount',
                available: 'Available',
                portfolio: 'Balances & Portfolio',
                cash: 'Cash:',
                closeBtn: 'Close / Sell',
                noPos: 'No open positions currently.',
                noHist: 'No trade history yet.',
                buySpotText: (name) => `BUY ${name} (SPOT)`,
                sellSpotText: (name) => `SELL ${name} (SPOT)`,
                buyLongText: 'Open Buy / Long Position',
                sellShortText: 'Open Sell / Short Position',
                signalBuy: 'STRONG BUY',
                signalSell: 'STRONG SELL',
                signalNeutral: 'NEUTRAL / WAIT',
                reasonBuy: (rsi) => `RSI indicates oversold condition (${rsi}). Upward bounce likely.`,
                reasonSell: (rsi) => `RSI indicates overbought condition (${rsi}). Sell pressure expected.`,
                reasonNeutral: (rsi) => `RSI is balanced (${rsi}). No breakout signals.`
            }
        };

        // --- DATA & STATE MANAGEMENT ---
        const INITIAL_BALANCE = 1000; // 1 000 SEK base
        let state = {
            usdt: INITIAL_BALANCE, // Cash balance stored in base SEK
            holdings: { BTCUSDT: 0, ETHUSDT: 0, SOLUSDT: 0 },
            activeAsset: 'BTCUSDT',
            tradeMode: 'MARGIN',
            orderSide: 'BUY',
            leverage: 1,
            timeframe: '1m',
            currency: 'SEK',
            lang: 'sv',
            aiBotActive: false,
            positions: [],
            history: [],
            aiLogs: []
        };

        // Helper functions to save and load state from localStorage
        function saveStateToStorage() {
            try {
                localStorage.setItem(STORAGE_KEY, JSON.stringify(state));
            } catch (e) {
                console.error("Kunde inte spara till localStorage:", e);
            }
        }

        function loadStateFromStorage() {
            try {
                const saved = localStorage.getItem(STORAGE_KEY);
                if (saved) {
                    const parsed = JSON.parse(saved);
                    state = { ...state, ...parsed };
                }
            } catch (e) {
                console.error("Kunde inte läsa från localStorage:", e);
            }
        }

        const assetConfigs = {
            BTCUSDT: { name: 'BTC', basePrice: 675000, volMultiplier: 3.5 },
            ETHUSDT: { name: 'ETH', basePrice: 35000, volMultiplier: 2.5 },
            SOLUSDT: { name: 'SOL', basePrice: 1500, volMultiplier: 1.8 }
        };

        let candleData = { BTCUSDT: [], ETHUSDT: [], SOLUSDT: [] };
        let currentPrices = { BTCUSDT: 675000.00, ETHUSDT: 35000.00, SOLUSDT: 1500.00 };

        function formatCurrency(valueInSek) {
            const curr = currencyRates[state.currency];
            const converted = valueInSek * curr.rate;
            const formatted = converted.toLocaleString(curr.locale, { minimumFractionDigits: 2, maximumFractionDigits: 2 });
            return curr.pos === 'before' ? `${curr.symbol}${formatted}` : `${formatted} ${curr.symbol}`;
        }

        // --- SEED INITIAL CANDLESTICK DATA BASED ON SELECTED TIMEFRAME ---
        function generateInitialCandles(symbol, timeframe) {
            const list = [];
            let price = currentPrices[symbol] || assetConfigs[symbol].basePrice;
            const now = Date.now();
            const config = timeframeConfigs[timeframe];
            const intervalMs = config.intervalMs;

            for (let i = config.candleCount; i >= 0; i--) {
                const time = now - (i * intervalMs);
                const volatility = price * config.volFactor;
                const change = (Math.random() - 0.495) * volatility;
                const open = price;
                const close = price + change;
                const high = Math.max(open, close) + Math.random() * (volatility * 0.4);
                const low = Math.min(open, close) - Math.random() * (volatility * 0.4);
                const volume = Math.floor(Math.random() * 50 + 10) * assetConfigs[symbol].volMultiplier;

                list.push({ time, open, high, low, close, volume });
                price = close;
            }
            currentPrices[symbol] = price;
            return list;
        }

        function reloadAllCandles() {
            Object.keys(assetConfigs).forEach(sym => {
                candleData[sym] = generateInitialCandles(sym, state.timeframe);
            });
        }

        reloadAllCandles();

        // --- REALTIME PRICE TICKER & SIMULATOR ---
        function updateMarketPrice() {
            const config = timeframeConfigs[state.timeframe];

            Object.keys(currentPrices).forEach(symbol => {
                const candles = candleData[symbol];
                if (!candles || candles.length === 0) return;
                const lastCandle = candles[candles.length - 1];
                
                // Increased baseline volatility with random price spike multiplier
                const spike = (Math.random() < 0.08) ? (Math.random() * 3 + 1.5) : 1;
                const volatility = currentPrices[symbol] * (config.volFactor * 0.4) * spike;
                const delta = (Math.random() - 0.495) * volatility;
                const newPrice = Math.max(0.01, currentPrices[symbol] + delta);
                
                currentPrices[symbol] = newPrice;

                lastCandle.close = newPrice;
                if (newPrice > lastCandle.high) lastCandle.high = newPrice;
                if (newPrice < lastCandle.low) lastCandle.low = newPrice;
                lastCandle.volume += Math.random() * 2.5;

                checkPositions(symbol);
            });

            // Periodically roll over to a new candle based on timeframe
            if (Math.random() < 0.2) {
                Object.keys(currentPrices).forEach(symbol => {
                    const candles = candleData[symbol];
                    const last = candles[candles.length - 1];
                    candles.push({
                        time: Date.now(),
                        open: last.close,
                        high: last.close,
                        low: last.close,
                        close: last.close,
                        volume: 1
                    });
                    if (candles.length > config.candleCount) candles.shift();
                });
            }

            evaluateAISignals();
            if (state.aiBotActive) runAIBotTrading();

            renderHeaderUI();
            renderOrderBook();
            renderChart();
            renderPositions();
            renderPortfolio();
        }

        // --- CANDLESTICK CANVAS RENDERER ---
        function renderChart() {
            if (!canvas || !ctx) return;

            const rect = canvas.parentElement.getBoundingClientRect();
            canvas.width = rect.width;
            canvas.height = rect.height;

            const width = canvas.width;
            const height = canvas.height;

            ctx.clearRect(0, 0, width, height);

            const candles = candleData[state.activeAsset];
            if (!candles || candles.length === 0) return;

            const latest = candles[candles.length - 1];
            document.getElementById('ohlcO').innerText = formatCurrency(latest.open);
            document.getElementById('ohlcH').innerText = formatCurrency(latest.high);
            document.getElementById('ohlcL').innerText = formatCurrency(latest.low);
            document.getElementById('ohlcC').innerText = formatCurrency(latest.close);

            const paddingBottom = 40;
            const chartHeight = height - paddingBottom;

            let minP = Math.min(...candles.map(c => c.low));
            let maxP = Math.max(...candles.map(c => c.high));

            const activeAssetPositions = state.positions.filter(p => p.asset === state.activeAsset);
            activeAssetPositions.forEach(p => {
                if (p.entryPrice < minP) minP = p.entryPrice;
                if (p.entryPrice > maxP) maxP = p.entryPrice;
            });

            const rangeP = (maxP - minP) || 1;

            minP -= rangeP * 0.05;
            maxP += rangeP * 0.05;
            const paddedRange = maxP - minP;

            ctx.strokeStyle = '#1e222d';
            ctx.lineWidth = 1;
            const gridRows = 5;
            for (let i = 0; i <= gridRows; i++) {
                const y = (chartHeight / gridRows) * i;
                ctx.beginPath();
                ctx.moveTo(0, y);
                ctx.lineTo(width, y);
                ctx.stroke();

                const priceVal = maxP - (paddedRange / gridRows) * i;
                ctx.fillStyle = '#848e9c';
                ctx.font = '10px monospace';
                ctx.fillText(formatCurrency(priceVal), width - 95, y - 4);
            }

            const candleWidth = (width - 100) / candles.length;
            const gap = Math.max(1, candleWidth * 0.2);
            const maxVol = Math.max(...candles.map(c => c.volume)) || 1;

            candles.forEach((c, index) => {
                const x = index * candleWidth + gap / 2;
                const isGreen = c.close >= c.open;
                const color = isGreen ? '#0ecb81' : '#f6465d';

                const openY = chartHeight - ((c.open - minP) / paddedRange) * chartHeight;
                const closeY = chartHeight - ((c.close - minP) / paddedRange) * chartHeight;
                const highY = chartHeight - ((c.high - minP) / paddedRange) * chartHeight;
                const lowY = chartHeight - ((c.low - minP) / paddedRange) * chartHeight;

                ctx.strokeStyle = color;
                ctx.lineWidth = 1.5;
                ctx.beginPath();
                ctx.moveTo(x + (candleWidth - gap) / 2, highY);
                ctx.lineTo(x + (candleWidth - gap) / 2, lowY);
                ctx.stroke();

                ctx.fillStyle = color;
                const bodyY = Math.min(openY, closeY);
                const bodyH = Math.max(2, Math.abs(closeY - openY));
                ctx.fillRect(x, bodyY, candleWidth - gap, bodyH);

                const volH = (c.volume / maxVol) * 35;
                ctx.fillStyle = isGreen ? 'rgba(14, 203, 129, 0.25)' : 'rgba(246, 70, 93, 0.25)';
                ctx.fillRect(x, height - volH, candleWidth - gap, volH);
            });

            // Draw open position buy/sell lines
            activeAssetPositions.forEach(pos => {
                const entryY = chartHeight - ((pos.entryPrice - minP) / paddedRange) * chartHeight;
                const isBuy = pos.type === 'BUY';
                const lineColor = isBuy ? '#0ecb81' : '#f6465d';

                ctx.strokeStyle = lineColor;
                ctx.lineWidth = 1.5;
                ctx.setLineDash([6, 3]);
                ctx.beginPath();
                ctx.moveTo(0, entryY);
                ctx.lineTo(width - 100, entryY);
                ctx.stroke();
                ctx.setLineDash([]);

                const typeLabel = isBuy ? (state.lang === 'sv' ? 'KÖP' : 'BUY') : (state.lang === 'sv' ? 'SÄLJ' : 'SELL');
                const labelText = `${typeLabel} @ ${formatCurrency(pos.entryPrice)}`;
                ctx.fillStyle = lineColor;
                ctx.fillRect(width - 100, entryY - 9, 98, 18);
                ctx.fillStyle = '#ffffff';
                ctx.font = 'bold 9px monospace';
                ctx.fillText(labelText, width - 97, entryY + 3);
            });

            // Draw current price line
            const lastPrice = latest.close;
            const lastPriceY = chartHeight - ((lastPrice - minP) / paddedRange) * chartHeight;

            ctx.strokeStyle = '#eac318';
            ctx.setLineDash([4, 4]);
            ctx.beginPath();
            ctx.moveTo(0, lastPriceY);
            ctx.lineTo(width - 100, lastPriceY);
            ctx.stroke();
            ctx.setLineDash([]);

            ctx.fillStyle = '#eac318';
            ctx.fillRect(width - 100, lastPriceY - 10, 98, 20);
            ctx.fillStyle = '#000000';
            ctx.font = 'bold 9.5px monospace';
            ctx.fillText(formatCurrency(lastPrice), width - 96, lastPriceY + 4);
        }

        // --- ORDER BOOK SIMULATOR ---
        function renderOrderBook() {
            const curPrice = currentPrices[state.activeAsset];
            const asksContainer = document.getElementById('orderBookAsks');
            const bidsContainer = document.getElementById('orderBookBids');
            
            let asksHTML = '';
            let bidsHTML = '';

            for (let i = 6; i >= 1; i--) {
                const price = curPrice + (i * (curPrice * 0.0003));
                const amount = (Math.random() * 1.5 + 0.1).toFixed(3);
                asksHTML += `
                    <div class="flex justify-between items-center px-1 text-trade-red hover:bg-trade-border/30 rounded cursor-pointer">
                        <span>${formatCurrency(price)}</span>
                        <span class="text-gray-400">${amount}</span>
                    </div>`;
            }

            for (let i = 1; i <= 6; i++) {
                const price = curPrice - (i * (curPrice * 0.0003));
                const amount = (Math.random() * 1.5 + 0.1).toFixed(3);
                bidsHTML += `
                    <div class="flex justify-between items-center px-1 text-trade-green hover:bg-trade-border/30 rounded cursor-pointer">
                        <span>${formatCurrency(price)}</span>
                        <span class="text-gray-400">${amount}</span>
                    </div>`;
            }

            asksContainer.innerHTML = asksHTML;
            bidsContainer.innerHTML = bidsHTML;
            document.getElementById('obCurrentPrice').innerText = formatCurrency(curPrice);
        }

        // --- AI SIGNALS CALCULATOR ENGINE ---
        function evaluateAISignals() {
            const candles = candleData[state.activeAsset];
            if (!candles || candles.length < 14) return;

            let gains = 0, losses = 0;
            for (let i = candles.length - 14; i < candles.length; i++) {
                const diff = candles[i].close - candles[i - 1].close;
                if (diff >= 0) gains += diff;
                else losses += Math.abs(diff);
            }
            const avgGain = gains / 14;
            const avgLoss = losses / 14 || 1;
            const rs = avgGain / avgLoss;
            const rsi = (100 - (100 / (1 + rs))).toFixed(1);

            document.getElementById('rsiValue').innerText = rsi;

            const t = translations[state.lang];
            const signalText = document.getElementById('aiSignalText');
            const signalReason = document.getElementById('aiSignalReason');
            const rsiEl = document.getElementById('rsiValue');

            if (rsi < 35) {
                signalText.innerText = t.signalBuy;
                signalText.className = "text-lg font-black text-trade-green tracking-wide";
                signalReason.innerText = t.reasonBuy(rsi);
                rsiEl.className = "font-bold text-trade-green mt-0.5";
            } else if (rsi > 65) {
                signalText.innerText = t.signalSell;
                signalText.className = "text-lg font-black text-trade-red tracking-wide";
                signalReason.innerText = t.reasonSell(rsi);
                rsiEl.className = "font-bold text-trade-red mt-0.5";
            } else {
                signalText.innerText = t.signalNeutral;
                signalText.className = "text-lg font-black text-yellow-500 tracking-wide";
                signalReason.innerText = t.reasonNeutral(rsi);
                rsiEl.className = "font-bold text-yellow-500 mt-0.5";
            }
        }

        function runAIBotTrading() {
            const now = Date.now();
            if (now - lastAiTradeTime < 8000) return;

            const rsi = parseFloat(document.getElementById('rsiValue').innerText);
            const curPrice = currentPrices[state.activeAsset];
            if (rsi < 32 && state.usdt >= 200) {
                executeOrder('BUY', state.activeAsset, 250, curPrice, 2);
                logAIAction(`[AI BOT] ${state.lang === 'sv' ? 'Öppnade KÖP' : 'Opened BUY'} (${state.activeAsset}) @ ${formatCurrency(curPrice)}`);
                lastAiTradeTime = now;
            }
            else if (rsi > 68 && state.positions.length > 0) {
                const posToClose = state.positions.find(p => p.type === 'BUY');
                if (posToClose) {
                    closePosition(posToClose.id);
                    logAIAction(`[AI BOT] ${state.lang === 'sv' ? 'Stängde position' : 'Closed position'} (${state.activeAsset}) @ ${formatCurrency(curPrice)}`);
                    lastAiTradeTime = now;
                }
            }
        }
        let lastAiTradeTime = 0;

        function logAIAction(message) {
            const logBox = document.getElementById('tabLogs');
            const entry = document.createElement('div');
            const timestamp = new Date().toLocaleTimeString();
            entry.innerText = `[${timestamp}] ${message}`;
            logBox.prepend(entry);
        }

        // --- SPOT & MARGIN TRADING ENGINE ---
        function executeOrder(side, asset, amountValInSek, price, leverage) {
            const assetName = assetConfigs[asset].name;

            if (state.tradeMode === 'SPOT') {
                if (side === 'BUY') {
                    if (amountValInSek <= 0 || amountValInSek > state.usdt) {
                        alert(state.lang === 'sv' ? "Otillräckligt saldo!" : "Insufficient balance!");
                        return;
                    }
                    const boughtCrypto = amountValInSek / price;
                    state.usdt -= amountValInSek;
                    state.holdings[asset] += boughtCrypto;

                    state.history.unshift({
                        time: new Date().toLocaleTimeString(),
                        asset: asset,
                        type: 'SPOT KÖP',
                        entryPrice: price,
                        exitPrice: price,
                        pnlUsdt: 0,
                        pnlPercentage: 0
                    });
                } else {
                    const cryptoAmount = amountValInSek; // In spot sell, amount represents crypto quantity
                    const currentHolding = state.holdings[asset];
                    if (cryptoAmount <= 0 || cryptoAmount > currentHolding) {
                        alert(state.lang === 'sv' ? `Otillräckligt ${assetName}-innehav!` : `Insufficient ${assetName} balance!`);
                        return;
                    }
                    const soldValueSek = cryptoAmount * price;
                    state.holdings[asset] -= cryptoAmount;
                    state.usdt += soldValueSek;

                    state.history.unshift({
                        time: new Date().toLocaleTimeString(),
                        asset: asset,
                        type: 'SPOT SÄLJ',
                        entryPrice: price,
                        exitPrice: price,
                        pnlUsdt: 0,
                        pnlPercentage: 0
                    });
                }
            } 
            else {
                if (amountValInSek <= 0 || amountValInSek > state.usdt) {
                    alert(state.lang === 'sv' ? "Otillräckligt saldo för denna position!" : "Insufficient margin balance!");
                    return;
                }

                state.usdt -= amountValInSek;
                const sizeInCrypto = (amountValInSek * leverage) / price;

                const newPosition = {
                    id: Date.now() + Math.random(),
                    asset: asset,
                    type: side,
                    amountUsdt: amountValInSek,
                    entryPrice: price,
                    size: sizeInCrypto,
                    leverage: leverage,
                    timestamp: new Date().toLocaleTimeString()
                };

                state.positions.push(newPosition);
            }

            saveStateToStorage();
            renderPositions();
            renderHistory();
            renderPortfolio();
        }

        function closePosition(positionId) {
            const index = state.positions.findIndex(p => p.id === positionId);
            if (index === -1) return;

            const pos = state.positions[index];
            const currentPrice = currentPrices[pos.asset];
            
            let priceDiff = currentPrice - pos.entryPrice;
            if (pos.type === 'SELL') priceDiff = -priceDiff;

            const pnlPercentage = (priceDiff / pos.entryPrice) * pos.leverage;
            const pnlUsdt = pos.amountUsdt * pnlPercentage;
            const returnUsdt = pos.amountUsdt + pnlUsdt;

            state.usdt = Math.max(0, state.usdt + returnUsdt);

            state.history.unshift({
                time: new Date().toLocaleTimeString(),
                asset: pos.asset,
                type: pos.type === 'BUY' ? 'LONG (CLOSED)' : 'SHORT (CLOSED)',
                entryPrice: pos.entryPrice,
                exitPrice: currentPrice,
                pnlUsdt: pnlUsdt,
                pnlPercentage: pnlPercentage * 100
            });

            state.positions.splice(index, 1);

            saveStateToStorage();
            renderPositions();
            renderHistory();
            renderPortfolio();
        }

        function closePosition(positionId) {
            const index = state.positions.findIndex(p => p.id === positionId);
            if (index === -1) return;

            const pos = state.positions[index];
            const currentPrice = currentPrices[pos.asset];
            
            let priceDiff = currentPrice - pos.entryPrice;
            if (pos.type === 'SELL') priceDiff = -priceDiff;

            const pnlPercentage = (priceDiff / pos.entryPrice) * pos.leverage;
            const pnlUsdt = pos.amountUsdt * pnlPercentage;
            const returnUsdt = pos.amountUsdt + pnlUsdt;

            state.usdt = Math.max(0, state.usdt + returnUsdt);

            state.history.unshift({
                time: new Date().toLocaleTimeString(),
                asset: pos.asset,
                type: pos.type === 'BUY' ? 'LONG (CLOSED)' : 'SHORT (CLOSED)',
                entryPrice: pos.entryPrice,
                exitPrice: currentPrice,
                pnlUsdt: pnlUsdt,
                pnlPercentage: pnlPercentage * 100
            });

            state.positions.splice(index, 1);

            renderPositions();
            renderHistory();
            renderPortfolio();
        }

        function checkPositions(symbol) {
            const curPrice = currentPrices[symbol];
            state.positions.forEach(pos => {
                if (pos.asset !== symbol) return;

                let diff = curPrice - pos.entryPrice;
                if (pos.type === 'SELL') diff = -diff;

                const lossPct = (diff / pos.entryPrice) * pos.leverage;
                if (lossPct <= -0.90) { 
                    logAIAction(`[LIQUIDATION] ${pos.asset} position closed due to high loss.`);
                    closePosition(pos.id);
                }
            });
        }

        // --- RENDER UI & LANGUAGE TRANSLATIONS ---
        function applyLanguage() {
            const t = translations[state.lang];
            
            document.getElementById('lblTotalBalance').innerText = t.totalVal;
            document.getElementById('lblAddFundsBtn').innerText = t.addFundsBtn;
            document.getElementById('lblResetBtn').innerText = t.resetBtn;
            document.getElementById('lblAiAnalysis').innerHTML = `<i data-lucide="sparkles" class="w-4 h-4"></i> ${t.aiAnalysis}`;
            document.getElementById('aiConfidence').innerText = `87% ${t.confidence}`;
            document.getElementById('lblTrend').innerText = t.trend;
            document.getElementById('lblOrderBook').innerText = t.orderBook;
            document.getElementById('lblSpread').innerText = `${t.spread}: 0.01%`;
            document.getElementById('lblTimeframe').innerText = t.timeframe;
            document.getElementById('lblOpenPositions').innerText = t.openPos;
            document.getElementById('lblOrderHistory').innerText = t.orderHist;
            document.getElementById('lblAiLogs').innerText = t.aiLogs;

            document.getElementById('thAsset').innerText = t.thAsset;
            document.getElementById('thType').innerText = t.thType;
            document.getElementById('thVolume').innerText = t.thVolume;
            document.getElementById('thEntry').innerText = t.thEntry;
            document.getElementById('thMarket').innerText = t.thMarket;
            document.getElementById('thLeverage').innerText = t.thLeverage;
            document.getElementById('thPnl').innerText = t.thPnl;
            document.getElementById('thAction').innerText = t.thAction;

            document.getElementById('thTime').innerText = t.thTime;
            document.getElementById('thHAsset').innerText = t.thAsset;
            document.getElementById('thHType').innerText = t.thType;
            document.getElementById('thHEntry').innerText = t.thEntry;
            document.getElementById('thHExit').innerText = t.thMarket;
            document.getElementById('thHNet').innerText = t.thNet;

            document.getElementById('lblTradingMode').innerText = t.tradeMode;
            document.getElementById('orderTypeBuy').innerText = t.buyLong;
            document.getElementById('orderTypeSell').innerText = t.sellShort;
            document.getElementById('lblLeverage').innerText = t.leverage;
            document.getElementById('lblAvailable').innerText = t.available;
            document.getElementById('lblPortfolioHeading').innerText = t.portfolio;
            document.getElementById('lblCashBalance').innerText = t.cash;

            updateOrderUI();
            evaluateAISignals();
            lucide.createIcons();
        }

        function renderHeaderUI() {
            const price = currentPrices[state.activeAsset];
            const candles = candleData[state.activeAsset];
            const startPrice = candles[0]?.open || price;
            const changePct = (((price - startPrice) / startPrice) * 100).toFixed(2);

            const hpEl = document.getElementById('headerPrice');
            const hcEl = document.getElementById('headerChange');

            hpEl.innerText = formatCurrency(price);
            if (changePct >= 0) {
                hcEl.innerText = `+${changePct}%`;
                hcEl.className = "text-xs font-medium text-trade-green";
                hpEl.className = "text-base font-bold text-trade-green";
            } else {
                hcEl.innerText = `${changePct}%`;
                hcEl.className = "text-xs font-medium text-trade-red";
                hpEl.className = "text-base font-bold text-trade-red";
            }

            let spotCryptoVal = 0;
            Object.keys(state.holdings).forEach(asset => {
                spotCryptoVal += state.holdings[asset] * currentPrices[asset];
            });

            let openPositionsVal = 0;
            state.positions.forEach(pos => {
                const cPrice = currentPrices[pos.asset];
                let diff = cPrice - pos.entryPrice;
                if (pos.type === 'SELL') diff = -diff;
                const pnl = pos.amountUsdt * ((diff / pos.entryPrice) * pos.leverage);
                openPositionsVal += (pos.amountUsdt + pnl);
            });

            const totalPortfolioSek = state.usdt + spotCryptoVal + openPositionsVal;
            document.getElementById('headerTotalBalance').innerText = formatCurrency(totalPortfolioSek);

            const availEl = document.getElementById('availUsdt');
            if (state.tradeMode === 'SPOT' && state.orderSide === 'SELL') {
                const currentCrypto = state.holdings[state.activeAsset];
                availEl.innerText = `${currentCrypto.toFixed(4)} ${assetConfigs[state.activeAsset].name}`;
            } else {
                availEl.innerText = formatCurrency(state.usdt);
            }
        }

        function renderPositions() {
            const tbody = document.getElementById('positionsTableBody');
            document.getElementById('posCount').innerText = state.positions.length;

            const t = translations[state.lang];

            if (state.positions.length === 0) {
                tbody.innerHTML = `<tr><td colspan="8" class="text-center py-6 text-trade-textMuted">${t.noPos}</td></tr>`;
                return;
            }

            tbody.innerHTML = state.positions.map(pos => {
                const curPrice = currentPrices[pos.asset];
                let diff = curPrice - pos.entryPrice;
                if (pos.type === 'SELL') diff = -diff;

                const pnlUsdt = pos.amountUsdt * ((diff / pos.entryPrice) * pos.leverage);
                const pnlPct = ((diff / pos.entryPrice) * pos.leverage) * 100;
                const isProfitable = pnlUsdt >= 0;
                const colorClass = isProfitable ? 'text-trade-green' : 'text-trade-red';

                return `
                    <tr class="border-b border-trade-border/40 hover:bg-trade-dark/30">
                        <td class="p-2 font-bold text-white">${pos.asset}</td>
                        <td class="p-2 font-semibold ${pos.type === 'BUY' ? 'text-trade-green' : 'text-trade-red'}">${pos.type === 'BUY' ? 'LONG' : 'SHORT'}</td>
                        <td class="p-2">${pos.size.toFixed(4)}</td>
                        <td class="p-2">${formatCurrency(pos.entryPrice)}</td>
                        <td class="p-2">${formatCurrency(curPrice)}</td>
                        <td class="p-2"><span class="bg-trade-border px-1.5 py-0.5 rounded text-[10px]">${pos.leverage}x</span></td>
                        <td class="p-2 font-bold ${colorClass}">
                            ${isProfitable ? '+' : ''}${formatCurrency(pnlUsdt)} (${isProfitable ? '+' : ''}${pnlPct.toFixed(2)}%)
                        </td>
                        <td class="p-2 text-right">
                            <button onclick="closePosition(${pos.id})" class="bg-trade-red/20 hover:bg-trade-red text-trade-red hover:text-white px-2 py-1 rounded text-[11px] font-sans transition">${t.closeBtn}</button>
                        </td>
                    </tr>
                `;
            }).join('');
        }

        function renderHistory() {
            const tbody = document.getElementById('historyTableBody');
            const t = translations[state.lang];

            if (state.history.length === 0) {
                tbody.innerHTML = `<tr><td colspan="6" class="text-center py-6 text-trade-textMuted">${t.noHist}</td></tr>`;
                return;
            }

            tbody.innerHTML = state.history.map(item => {
                const isProfitable = item.pnlUsdt >= 0;
                const colorClass = isProfitable ? 'text-trade-green' : 'text-trade-red';
                return `
                    <tr class="border-b border-trade-border/40">
                        <td class="p-2 text-trade-textMuted">${item.time}</td>
                        <td class="p-2 font-bold text-white">${item.asset}</td>
                        <td class="p-2 ${item.type.includes('KÖP') || item.type.includes('BUY') ? 'text-trade-green' : 'text-trade-red'}">${item.type}</td>
                        <td class="p-2">${formatCurrency(item.entryPrice)}</td>
                        <td class="p-2">${formatCurrency(item.exitPrice)}</td>
                        <td class="p-2 font-bold ${colorClass}">${isProfitable ? '+' : ''}${formatCurrency(item.pnlUsdt)} (${isProfitable ? '+' : ''}${item.pnlPercentage.toFixed(2)}%)</td>
                    </tr>
                `;
            }).join('');
        }

        function renderPortfolio() {
            document.getElementById('walletUsdt').innerText = formatCurrency(state.usdt);
            document.getElementById('walletBtc').innerText = `${state.holdings.BTCUSDT.toFixed(4)} BTC`;
            document.getElementById('walletEth').innerText = `${state.holdings.ETHUSDT.toFixed(4)} ETH`;
            document.getElementById('walletSol').innerText = `${state.holdings.SOLUSDT.toFixed(4)} SOL`;
        }

        function updateOrderUI() {
            const execBtn = document.getElementById('executeTradeBtn');
            const execBtnText = document.getElementById('executeBtnText');
            const amountLabel = document.getElementById('amountInputLabel');
            const unitLabel = document.getElementById('amountUnitLabel');
            const currSymbol = currencyRates[state.currency].symbol;

            const t = translations[state.lang];
            const isBuy = state.orderSide === 'BUY';
            const isSpot = state.tradeMode === 'SPOT';

            if (isBuy) {
                document.getElementById('orderTypeBuy').className = "py-2 text-xs font-bold rounded-md bg-trade-green text-white shadow-lg transition";
                document.getElementById('orderTypeSell').className = "py-2 text-xs font-bold rounded-md text-trade-textMuted hover:text-white transition";
                execBtn.className = "w-full bg-trade-green hover:bg-trade-greenHover text-white font-bold py-3 rounded-lg text-sm tracking-wide shadow-lg transition uppercase flex items-center justify-center space-x-2 mb-6";
                execBtnText.innerText = isSpot ? t.buySpotText(assetConfigs[state.activeAsset].name) : t.buyLongText;
                amountLabel.innerText = `${t.amount} (${state.currency})`;
                unitLabel.innerText = currSymbol;
            } else {
                document.getElementById('orderTypeSell').className = "py-2 text-xs font-bold rounded-md bg-trade-red text-white shadow-lg transition";
                document.getElementById('orderTypeBuy').className = "py-2 text-xs font-bold rounded-md text-trade-textMuted hover:text-white transition";
                execBtn.className = "w-full bg-trade-red hover:bg-trade-redHover text-white font-bold py-3 rounded-lg text-sm tracking-wide shadow-lg transition uppercase flex items-center justify-center space-x-2 mb-6";
                execBtnText.innerText = isSpot ? t.sellSpotText(assetConfigs[state.activeAsset].name) : t.sellShortText;
                
                if (isSpot) {
                    amountLabel.innerText = `${t.amount} (${assetConfigs[state.activeAsset].name})`;
                    unitLabel.innerText = assetConfigs[state.activeAsset].name;
                } else {
                    amountLabel.innerText = `${t.amount} (${state.currency})`;
                    unitLabel.innerText = currSymbol;
                }
            }
            renderHeaderUI();
        }

        // --- EVENT LISTENERS ---
        document.addEventListener('DOMContentLoaded', () => {
            canvas = document.getElementById('candleCanvas');
            ctx = canvas.getContext('2d');

            // Load persisted state from localStorage
            loadStateFromStorage();

            // Set select values to match restored state
            document.getElementById('currencySelect').value = state.currency;
            document.getElementById('langSelect').value = state.lang;
            document.getElementById('assetSelect').value = state.activeAsset;
            document.getElementById('aiBotToggle').checked = state.aiBotActive;
            document.getElementById('leverageRange').value = state.leverage;
            document.getElementById('leverageVal').innerText = `${state.leverage}x`;

            // Set active timeframe button visually
            document.querySelectorAll('.tf-btn').forEach(btn => {
                if (btn.dataset.tf === state.timeframe) {
                    btn.className = "tf-btn text-xs px-2.5 py-1 rounded bg-trade-dark border border-yellow-500 text-yellow-500 font-semibold";
                } else {
                    btn.className = "tf-btn text-xs px-2.5 py-1 rounded bg-trade-dark text-trade-textMuted hover:text-white";
                }
            });

            // Set active trading mode visually
            const marginBtn = document.getElementById('modeMarginBtn');
            const spotBtn = document.getElementById('modeSpotBtn');
            const levGroup = document.getElementById('leverageGroup');

            if (state.tradeMode === 'SPOT') {
                spotBtn.className = "py-1.5 rounded font-bold bg-trade-border text-white";
                marginBtn.className = "py-1.5 rounded font-bold text-trade-textMuted hover:text-white";
                levGroup.classList.add('hidden');
            } else {
                marginBtn.className = "py-1.5 rounded font-bold bg-trade-border text-white";
                spotBtn.className = "py-1.5 rounded font-bold text-trade-textMuted hover:text-white";
                levGroup.classList.remove('hidden');
            }

            lucide.createIcons();

            // Currency Selector Event
            document.getElementById('currencySelect').addEventListener('change', (e) => {
                state.currency = e.target.value;
                saveStateToStorage();
                renderChart();
                renderOrderBook();
                renderHeaderUI();
                renderPositions();
                renderHistory();
                renderPortfolio();
                updateOrderUI();
            });

            // Language Selector Event
            document.getElementById('langSelect').addEventListener('change', (e) => {
                state.lang = e.target.value;
                saveStateToStorage();
                applyLanguage();
            });

            // Timeframe Buttons Event (Functional timeframe switching)
            document.querySelectorAll('.tf-btn').forEach(btn => {
                btn.addEventListener('click', () => {
                    document.querySelectorAll('.tf-btn').forEach(b => {
                        b.className = "tf-btn text-xs px-2.5 py-1 rounded bg-trade-dark text-trade-textMuted hover:text-white";
                    });
                    btn.className = "tf-btn text-xs px-2.5 py-1 rounded bg-trade-dark border border-yellow-500 text-yellow-500 font-semibold";
                    
                    state.timeframe = btn.dataset.tf;
                    saveStateToStorage();
                    reloadAllCandles();
                    renderChart();
                });
            });

            // Asset selector
            document.getElementById('assetSelect').addEventListener('change', (e) => {
                state.activeAsset = e.target.value;
                saveStateToStorage();
                renderChart();
                renderOrderBook();
                renderHeaderUI();
                updateOrderUI();
            });

            // Margin vs Spot switcher
            marginBtn.addEventListener('click', () => {
                state.tradeMode = 'MARGIN';
                marginBtn.className = "py-1.5 rounded font-bold bg-trade-border text-white";
                spotBtn.className = "py-1.5 rounded font-bold text-trade-textMuted hover:text-white";
                levGroup.classList.remove('hidden');
                saveStateToStorage();
                updateOrderUI();
            });

            spotBtn.addEventListener('click', () => {
                state.tradeMode = 'SPOT';
                spotBtn.className = "py-1.5 rounded font-bold bg-trade-border text-white";
                marginBtn.className = "py-1.5 rounded font-bold text-trade-textMuted hover:text-white";
                levGroup.classList.add('hidden');
                saveStateToStorage();
                updateOrderUI();
            });

            // Buy / Sell Tabs
            document.getElementById('orderTypeBuy').addEventListener('click', () => {
                state.orderSide = 'BUY';
                saveStateToStorage();
                updateOrderUI();
            });

            document.getElementById('orderTypeSell').addEventListener('click', () => {
                state.orderSide = 'SELL';
                saveStateToStorage();
                updateOrderUI();
            });

            // Leverage range
            document.getElementById('leverageRange').addEventListener('input', (e) => {
                state.leverage = parseInt(e.target.value);
                document.getElementById('leverageVal').innerText = `${state.leverage}x`;
                saveStateToStorage();
            });

            // AI Bot Switch
            document.getElementById('aiBotToggle').addEventListener('change', (e) => {
                state.aiBotActive = e.target.checked;
                saveStateToStorage();
                logAIAction(state.aiBotActive ? "AI Auto-Trader active." : "AI Auto-Trader disabled.");
            });

            // Add Funds (+10 000 SEK)
            document.getElementById('addFundsBtn').addEventListener('click', () => {
                const addAmountSek = 10000;
                state.usdt += addAmountSek;
                saveStateToStorage();
                renderHeaderUI();
                renderPortfolio();
                logAIAction(`+${formatCurrency(addAmountSek)} tillagda i plånboken.`);
            });

            // Reset Balance
            document.getElementById('resetBtn').addEventListener('click', () => {
                state.usdt = INITIAL_BALANCE;
                state.holdings = { BTCUSDT: 0, ETHUSDT: 0, SOLUSDT: 0 };
                state.positions = [];
                state.history = [];
                localStorage.removeItem(STORAGE_KEY);
                renderPositions();
                renderHistory();
                renderPortfolio();
                logAIAction("Account balance reset.");
            });

            // --- EXECUTE TRADE BUTTON CLICK LISTENER ---
            document.getElementById('executeTradeBtn').addEventListener('click', () => {
                const amountInput = document.getElementById('tradeAmountInput');
                const rawVal = parseFloat(amountInput.value);

                if (isNaN(rawVal) || rawVal <= 0) {
                    alert(state.lang === 'sv' ? "Ange ett giltigt belopp!" : "Please enter a valid amount!");
                    return;
                }

                const price = currentPrices[state.activeAsset];
                let amountInSek = rawVal;

                // Handle Currency conversion if inputting in non-SEK currency
                const currRate = currencyRates[state.currency].rate;

                if (state.tradeMode === 'SPOT' && state.orderSide === 'SELL') {
                    // For Spot Sell, amount input is in Crypto quantity
                    executeOrder('SELL', state.activeAsset, rawVal, price, 1);
                } else {
                    // Convert entered currency amount back to base SEK
                    amountInSek = rawVal / currRate;
                    executeOrder(state.orderSide, state.activeAsset, amountInSek, price, state.leverage);
                }

                amountInput.value = '';
            });

            // --- QUICK PERCENTAGE BUTTONS CLICK LISTENERS ---
            document.querySelectorAll('.pct-btn').forEach(btn => {
                btn.addEventListener('click', () => {
                    const pct = parseFloat(btn.dataset.pct) / 100;
                    const amountInput = document.getElementById('tradeAmountInput');
                    const currRate = currencyRates[state.currency].rate;

                    if (state.tradeMode === 'SPOT' && state.orderSide === 'SELL') {
                        // Calculate percentage of crypto holding
                        const holding = state.holdings[state.activeAsset] || 0;
                        const calcAmount = (holding * pct).toFixed(4);
                        amountInput.value = calcAmount;
                    } else {
                        // Calculate percentage of cash balance in selected currency
                        const availSek = state.usdt;
                        const availInCurr = availSek * currRate;
                        const calcAmount = (availInCurr * pct).toFixed(2);
                        amountInput.value = calcAmount;
                    }
                });
            });

            // Apply loaded language and initial renders
            applyLanguage();
            reloadAllCandles();
            renderPositions();
            renderHistory();
            renderPortfolio();

            // Bottom Panel Tabs Switching
            const tabPosBtn = document.getElementById('tabPositionsBtn');
            const tabHisBtn = document.getElementById('tabHistoryBtn');
            const tabLogBtn = document.getElementById('tabLogsBtn');

            const tabPos = document.getElementById('tabPositions');
            const tabHis = document.getElementById('tabHistory');
            const tabLog = document.getElementById('tabLogs');

            function resetTabs() {
                [tabPosBtn, tabHisBtn, tabLogBtn].forEach(b => b.className = "py-2.5 px-4 text-trade-textMuted hover:text-white focus:outline-none");
                [tabPos, tabHis, tabLog].forEach(d => d.classList.add('hidden'));
            }

            tabPosBtn.addEventListener('click', () => {
                resetTabs();
                tabPosBtn.className = "py-2.5 px-4 text-yellow-500 border-b-2 border-yellow-500 font-bold focus:outline-none";
                tabPos.classList.remove('hidden');
            });

            tabHisBtn.addEventListener('click', () => {
                resetTabs();
                tabHisBtn.className = "py-2.5 px-4 text-yellow-500 border-b-2 border-yellow-500 font-bold focus:outline-none";
                tabHis.classList.remove('hidden');
            });

            tabLogBtn.addEventListener('click', () => {
                resetTabs();
                tabLogBtn.className = "py-2.5 px-4 text-yellow-500 border-b-2 border-yellow-500 font-bold focus:outline-none";
                tabLog.classList.remove('hidden');
            });

            window.addEventListener('resize', renderChart);
            setInterval(updateMarketPrice, 500);
        });
    </script>
</body>
</html>
