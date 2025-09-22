<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CFO Helper - Financial Scenario Simulator</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Inter', -apple-system, BlinkMacSystemFont, sans-serif;
            background: #0a0f1c;
            color: #ffffff;
            overflow-x: hidden;
        }

        .container {
            max-width: 1400px;
            margin: 0 auto;
            padding: 2rem;
        }

        .header {
            text-align: center;
            margin-bottom: 3rem;
            transform: perspective(1000px) rotateX(5deg);
        }

        .header h1 {
            font-size: 3rem;
            font-weight: 700;
            color: #ffffff;
            margin-bottom: 0.5rem;
            text-shadow: 0 4px 20px rgba(255, 255, 255, 0.1);
        }

        .header p {
            font-size: 1.2rem;
            color: #94a3b8;
            font-weight: 300;
        }

        .dashboard {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 2rem;
            margin-bottom: 3rem;
        }

        .card {
            background: rgba(15, 23, 42, 0.8);
            border: 1px solid rgba(51, 65, 85, 0.3);
            border-radius: 16px;
            padding: 2rem;
            backdrop-filter: blur(10px);
            transform: perspective(1000px) rotateY(-2deg);
            transition: all 0.3s ease;
            box-shadow: 
                0 8px 32px rgba(0, 0, 0, 0.3),
                inset 0 1px 0 rgba(255, 255, 255, 0.1);
        }

        .card:hover {
            transform: perspective(1000px) rotateY(0deg) translateY(-5px);
            box-shadow: 
                0 20px 40px rgba(0, 0, 0, 0.4),
                inset 0 1px 0 rgba(255, 255, 255, 0.2);
        }

        .card h3 {
            font-size: 1.5rem;
            margin-bottom: 1.5rem;
            color: #e2e8f0;
            display: flex;
            align-items: center;
            gap: 0.5rem;
        }

        .input-group {
            margin-bottom: 1.5rem;
        }

        .input-group label {
            display: block;
            margin-bottom: 0.5rem;
            color: #cbd5e1;
            font-weight: 500;
        }

        .input-group input, .input-group select {
            width: 100%;
            padding: 0.75rem 1rem;
            background: rgba(30, 41, 59, 0.5);
            border: 1px solid rgba(71, 85, 105, 0.5);
            border-radius: 8px;
            color: #ffffff;
            font-size: 1rem;
            transition: all 0.3s ease;
        }

        .input-group input:focus, .input-group select:focus {
            outline: none;
            border-color: #3b82f6;
            box-shadow: 0 0 0 3px rgba(59, 130, 246, 0.1);
            transform: translateZ(5px);
        }

        .button {
            background: linear-gradient(135deg, #1e40af 0%, #3b82f6 100%);
            color: white;
            border: none;
            padding: 0.75rem 1.5rem;
            border-radius: 8px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            transform: perspective(500px) rotateX(-5deg);
            box-shadow: 0 4px 15px rgba(59, 130, 246, 0.3);
        }

        .button:hover {
            transform: perspective(500px) rotateX(0deg) translateY(-2px);
            box-shadow: 0 8px 25px rgba(59, 130, 246, 0.4);
        }

        .results-section {
            background: rgba(15, 23, 42, 0.9);
            border: 1px solid rgba(51, 65, 85, 0.3);
            border-radius: 16px;
            padding: 2rem;
            margin-top: 2rem;
            transform: perspective(1000px) rotateX(2deg);
            backdrop-filter: blur(15px);
        }

        .metrics-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 1.5rem;
            margin-top: 2rem;
        }

        .metric-card {
            background: rgba(30, 41, 59, 0.6);
            border: 1px solid rgba(71, 85, 105, 0.3);
            border-radius: 12px;
            padding: 1.5rem;
            text-align: center;
            transform: perspective(500px) rotateY(5deg);
            transition: all 0.3s ease;
        }

        .metric-card:hover {
            transform: perspective(500px) rotateY(0deg) scale(1.05);
        }

        .metric-value {
            font-size: 2rem;
            font-weight: 700;
            color: #10b981;
            margin-bottom: 0.5rem;
        }

        .metric-label {
            color: #94a3b8;
            font-size: 0.9rem;
            text-transform: uppercase;
            letter-spacing: 0.5px;
        }

        .scenario-tabs {
            display: flex;
            gap: 1rem;
            margin-bottom: 2rem;
        }

        .tab {
            padding: 0.75rem 1.5rem;
            background: rgba(30, 41, 59, 0.5);
            border: 1px solid rgba(71, 85, 105, 0.3);
            border-radius: 8px;
            color: #cbd5e1;
            cursor: pointer;
            transition: all 0.3s ease;
            transform: perspective(500px) rotateX(-3deg);
        }

        .tab.active {
            background: rgba(59, 130, 246, 0.2);
            border-color: #3b82f6;
            color: #ffffff;
            transform: perspective(500px) rotateX(0deg);
        }

        .chart-container {
            background: rgba(30, 41, 59, 0.4);
            border-radius: 12px;
            padding: 2rem;
            margin-top: 2rem;
            min-height: 300px;
            display: flex;
            align-items: center;
            justify-content: center;
            transform: perspective(1000px) rotateX(-1deg);
        }

        .chart-placeholder {
            color: #64748b;
            font-size: 1.1rem;
            text-align: center;
        }

        @media (max-width: 768px) {
            .dashboard {
                grid-template-columns: 1fr;
            }
            
            .header h1 {
                font-size: 2rem;
            }
            
            .container {
                padding: 1rem;
            }
        }

        .icon {
            width: 24px;
            height: 24px;
            fill: currentColor;
        }

        .history-list {
            display: grid;
            gap: 1.5rem;
        }

        .history-item {
            background: rgba(30, 41, 59, 0.6);
            border: 1px solid rgba(71, 85, 105, 0.3);
            border-radius: 12px;
            padding: 1.5rem;
            transform: perspective(500px) rotateY(2deg);
            transition: all 0.3s ease;
            cursor: pointer;
        }

        .history-item:hover {
            transform: perspective(500px) rotateY(0deg) translateY(-3px);
            box-shadow: 0 8px 25px rgba(0, 0, 0, 0.3);
        }

        .history-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 1rem;
        }

        .history-scenario {
            background: rgba(59, 130, 246, 0.2);
            color: #3b82f6;
            padding: 0.25rem 0.75rem;
            border-radius: 6px;
            font-size: 0.8rem;
            font-weight: 600;
            text-transform: uppercase;
        }

        .history-date {
            color: #94a3b8;
            font-size: 0.9rem;
        }

        .history-metrics {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(120px, 1fr));
            gap: 1rem;
            margin-top: 1rem;
        }

        .history-metric {
            text-align: center;
        }

        .history-metric-value {
            font-size: 1.2rem;
            font-weight: 600;
            color: #10b981;
        }

        .history-metric-label {
            font-size: 0.8rem;
            color: #64748b;
            text-transform: uppercase;
        }

        .history-actions {
            display: flex;
            gap: 0.5rem;
            margin-top: 1rem;
        }

        .history-btn {
            padding: 0.5rem 1rem;
            border: none;
            border-radius: 6px;
            font-size: 0.8rem;
            cursor: pointer;
            transition: all 0.3s ease;
        }

        .history-btn.load {
            background: rgba(59, 130, 246, 0.2);
            color: #3b82f6;
            border: 1px solid rgba(59, 130, 246, 0.3);
        }

        .history-btn.delete {
            background: rgba(239, 68, 68, 0.2);
            color: #ef4444;
            border: 1px solid rgba(239, 68, 68, 0.3);
        }

        .history-btn:hover {
            transform: translateY(-1px);
        }

        .visual-card {
            background: rgba(15, 23, 42, 0.9);
            border: 1px solid rgba(51, 65, 85, 0.3);
            border-radius: 16px;
            padding: 2rem;
            margin-top: 2rem;
            backdrop-filter: blur(15px);
            transform: perspective(1000px) rotateX(2deg);
            transition: all 0.3s ease;
        }

        .visual-card:hover {
            transform: perspective(1000px) rotateX(0deg) translateY(-5px);
        }

        .visual-card h4 {
            color: #e2e8f0;
            font-size: 1.3rem;
            margin-bottom: 1.5rem;
            display: flex;
            align-items: center;
            gap: 0.5rem;
        }

        .timeline-container {
            display: flex;
            align-items: center;
            gap: 1rem;
            padding: 2rem 0;
            overflow-x: auto;
        }

        .timeline-item {
            display: flex;
            flex-direction: column;
            align-items: center;
            min-width: 120px;
            position: relative;
            opacity: 0;
            transform: translateY(20px);
            transition: all 0.5s ease;
        }

        .timeline-year {
            background: rgba(59, 130, 246, 0.2);
            color: #3b82f6;
            padding: 0.5rem 1rem;
            border-radius: 20px;
            font-weight: 600;
            margin-bottom: 1rem;
            border: 2px solid rgba(59, 130, 246, 0.3);
        }

        .timeline-value {
            background: linear-gradient(135deg, #10b981, #059669);
            color: white;
            padding: 1rem;
            border-radius: 12px;
            font-weight: 700;
            font-size: 1.1rem;
            text-align: center;
            box-shadow: 0 4px 15px rgba(16, 185, 129, 0.3);
            transform: perspective(500px) rotateX(-5deg);
            transition: all 0.3s ease;
        }

        .timeline-value:hover {
            transform: perspective(500px) rotateX(0deg) scale(1.05);
        }

        .timeline-connector {
            position: absolute;
            top: 50%;
            right: -0.5rem;
            width: 1rem;
            height: 3px;
            background: linear-gradient(90deg, #3b82f6, #10b981);
            border-radius: 2px;
        }

        .timeline-item:last-child .timeline-connector {
            display: none;
        }

        .risk-roi-container {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 3rem;
            padding: 2rem 0;
        }

        .gauge-visual {
            text-align: center;
        }

        .gauge-label {
            color: #cbd5e1;
            font-size: 1rem;
            margin-bottom: 1rem;
            text-transform: uppercase;
            letter-spacing: 1px;
        }

        .gauge-value {
            font-size: 2.5rem;
            font-weight: 700;
            margin-bottom: 1.5rem;
            text-shadow: 0 2px 10px rgba(255, 255, 255, 0.1);
        }

        .gauge-bar {
            width: 100%;
            height: 20px;
            background: rgba(71, 85, 105, 0.3);
            border-radius: 10px;
            overflow: hidden;
            position: relative;
        }

        .gauge-fill {
            height: 100%;
            border-radius: 10px;
            transition: all 1s ease;
            position: relative;
            overflow: hidden;
        }

        .gauge-fill::after {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.3), transparent);
            animation: shimmer 2s infinite;
        }

        @keyframes shimmer {
            0% { left: -100%; }
            100% { left: 100%; }
        }

        .cash-flow-visual {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 2rem;
            padding: 2rem 0;
        }

        .cash-flow-item {
            text-align: center;
            padding: 1.5rem;
            background: rgba(30, 41, 59, 0.6);
            border-radius: 12px;
            border: 1px solid rgba(71, 85, 105, 0.3);
            transform: perspective(500px) rotateY(5deg);
            transition: all 0.3s ease;
        }

        .cash-flow-item:hover {
            transform: perspective(500px) rotateY(0deg) scale(1.05);
        }

        .cash-flow-icon {
            font-size: 2rem;
            margin-bottom: 1rem;
        }

        .cash-flow-label {
            color: #94a3b8;
            font-size: 0.9rem;
            margin-bottom: 0.5rem;
            text-transform: uppercase;
            letter-spacing: 0.5px;
        }

        .cash-flow-value {
            color: #10b981;
            font-size: 1.5rem;
            font-weight: 700;
        }

        .cash-flow-bar {
            width: 100%;
            height: 8px;
            background: rgba(71, 85, 105, 0.3);
            border-radius: 4px;
            margin-top: 1rem;
            overflow: hidden;
        }

        .cash-flow-fill {
            height: 100%;
            background: linear-gradient(90deg, #10b981, #059669);
            border-radius: 4px;
            transition: width 1s ease;
        }

        .comparison-visual {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 2rem;
            padding: 2rem 0;
        }

        .comparison-item {
            text-align: center;
            padding: 2rem 1rem;
            border-radius: 16px;
            border: 2px solid;
            position: relative;
            overflow: hidden;
            transform: perspective(500px) rotateY(-5deg);
            transition: all 0.3s ease;
            opacity: 0;
        }

        .comparison-item:hover {
            transform: perspective(500px) rotateY(0deg) scale(1.05);
        }

        .comparison-item::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            opacity: 0.1;
            z-index: 0;
        }

        .comparison-item > * {
            position: relative;
            z-index: 1;
        }

        .comparison-conservative {
            border-color: #10b981;
            background: rgba(16, 185, 129, 0.1);
        }

        .comparison-conservative::before {
            background: #10b981;
        }

        .comparison-current {
            border-color: #3b82f6;
            background: rgba(59, 130, 246, 0.1);
        }

        .comparison-current::before {
            background: #3b82f6;
        }

        .comparison-aggressive {
            border-color: #f59e0b;
            background: rgba(245, 158, 11, 0.1);
        }

        .comparison-aggressive::before {
            background: #f59e0b;
        }

        .comparison-title {
            font-size: 1.2rem;
            font-weight: 700;
            margin-bottom: 1rem;
            text-transform: uppercase;
            letter-spacing: 1px;
        }

        .comparison-revenue {
            font-size: 2rem;
            font-weight: 700;
            margin-bottom: 0.5rem;
        }

        .comparison-growth {
            font-size: 1rem;
            margin-bottom: 1rem;
            opacity: 0.8;
        }

        .comparison-risk {
            display: inline-block;
            padding: 0.5rem 1rem;
            border-radius: 20px;
            font-size: 0.8rem;
            font-weight: 600;
            text-transform: uppercase;
        }

        .risk-low {
            background: rgba(16, 185, 129, 0.2);
            color: #10b981;
        }

        .risk-medium {
            background: rgba(245, 158, 11, 0.2);
            color: #f59e0b;
        }

        .risk-high {
            background: rgba(239, 68, 68, 0.2);
            color: #ef4444;
        }

        @media (max-width: 768px) {
            .risk-roi-container {
                grid-template-columns: 1fr;
                gap: 2rem;
            }
            
            .comparison-visual {
                grid-template-columns: 1fr;
            }
            
            .timeline-container {
                flex-direction: column;
                gap: 2rem;
            }
            
            .timeline-connector {
                display: none;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>CFO Helper</h1>
            <p>Interactive Financial Scenario Simulator</p>
        </div>

        <div class="scenario-tabs">
            <div class="tab active" onclick="switchScenario('revenue')">Revenue Growth</div>
            <div class="tab" onclick="switchScenario('cost')">Cost Optimization</div>
            <div class="tab" onclick="switchScenario('investment')">Investment Analysis</div>
            <div class="tab" onclick="toggleHistory()">📊 History</div>
        </div>

        <div class="dashboard">
            <div class="card">
                <h3>
                    <svg class="icon" viewBox="0 0 24 24">
                        <path d="M12 2L2 7v10c0 5.55 3.84 9.739 9 11 5.16-1.261 9-5.45 9-11V7l-10-5z"/>
                    </svg>
                    Scenario Parameters
                </h3>
                
                <div class="input-group">
                    <label for="currentRevenue">Current Annual Revenue (₹Cr)</label>
                    <input type="number" id="currentRevenue" value="100" min="0" step="0.1">
                </div>
                
                <div class="input-group">
                    <label for="growthRate">Growth Rate (%)</label>
                    <input type="number" id="growthRate" value="15" min="-50" max="100" step="0.1">
                </div>
                
                <div class="input-group">
                    <label for="timeHorizon">Time Horizon (Years)</label>
                    <select id="timeHorizon">
                        <option value="1">1 Year</option>
                        <option value="3" selected>3 Years</option>
                        <option value="5">5 Years</option>
                        <option value="10">10 Years</option>
                    </select>
                </div>
                
                <div class="input-group">
                    <label for="riskFactor">Risk Factor</label>
                    <select id="riskFactor">
                        <option value="low">Low Risk</option>
                        <option value="medium" selected>Medium Risk</option>
                        <option value="high">High Risk</option>
                    </select>
                </div>
                
                <button class="button" onclick="runSimulation()">Run Simulation</button>
                <button class="button" onclick="downloadReport()" style="margin-top: 1rem; background: linear-gradient(135deg, #059669 0%, #10b981 100%);">Download Report</button>
            </div>

            <div class="card">
                <h3>
                    <svg class="icon" viewBox="0 0 24 24">
                        <path d="M3 13h8V3H3v10zm0 8h8v-6H3v6zm10 0h8V11h-8v10zm0-18v6h8V3h-8z"/>
                    </svg>
                    Market Conditions
                </h3>
                
                <div class="input-group">
                    <label for="marketGrowth">Market Growth Rate (%)</label>
                    <input type="number" id="marketGrowth" value="8" min="-20" max="50" step="0.1">
                </div>
                
                <div class="input-group">
                    <label for="competition">Competition Level</label>
                    <select id="competition">
                        <option value="low">Low</option>
                        <option value="medium" selected>Medium</option>
                        <option value="high">High</option>
                    </select>
                </div>
                
                <div class="input-group">
                    <label for="economicCondition">Economic Outlook</label>
                    <select id="economicCondition">
                        <option value="recession">Recession</option>
                        <option value="stable" selected>Stable</option>
                        <option value="growth">Growth</option>
                        <option value="boom">Boom</option>
                    </select>
                </div>
                
                <div class="input-group">
                    <label for="inflationRate">Inflation Rate (%)</label>
                    <input type="number" id="inflationRate" value="3.2" min="0" max="20" step="0.1">
                </div>
            </div>
        </div>

        <!-- History Panel -->
        <div id="historyPanel" class="results-section" style="display: none;">
            <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 2rem;">
                <h3>Simulation History</h3>
                <button class="button" onclick="clearHistory()" style="background: linear-gradient(135deg, #dc2626 0%, #ef4444 100%); padding: 0.5rem 1rem;">Clear History</button>
            </div>
            
            <div id="historyList" class="history-list">
                <div class="history-empty">
                    <div style="text-align: center; color: #64748b; padding: 3rem;">
                        <div style="font-size: 3rem; margin-bottom: 1rem;">📈</div>
                        <div style="font-size: 1.2rem; margin-bottom: 0.5rem;">No simulation history yet</div>
                        <div>Run some simulations to see them here</div>
                    </div>
                </div>
            </div>
        </div>

        <div class="results-section" id="resultsSection">
            <h3>Simulation Results</h3>
            
            <div class="metrics-grid">
                <div class="metric-card">
                    <div class="metric-value" id="projectedRevenue">₹0Cr</div>
                    <div class="metric-label">Projected Revenue</div>
                </div>
                
                <div class="metric-card">
                    <div class="metric-value" id="totalGrowth">0%</div>
                    <div class="metric-label">Total Growth</div>
                </div>
                
                <div class="metric-card">
                    <div class="metric-value" id="riskScore">0</div>
                    <div class="metric-label">Risk Score</div>
                </div>
                
                <div class="metric-card">
                    <div class="metric-value" id="roi">0%</div>
                    <div class="metric-label">Expected ROI</div>
                </div>
                
                <div class="metric-card">
                    <div class="metric-value" id="breakeven">0</div>
                    <div class="metric-label">Breakeven (Months)</div>
                </div>
                
                <div class="metric-card">
                    <div class="metric-value" id="cashFlow">₹0Cr</div>
                    <div class="metric-label">Net Cash Flow</div>
                </div>
            </div>

            <!-- Revenue Projection Visual -->
            <div class="visual-card">
                <h4>📈 Revenue Projection Timeline</h4>
                <div class="timeline-container" id="revenueTimeline">
                    <!-- Timeline will be generated here -->
                </div>
            </div>
            
            <!-- Risk & ROI Visual -->
            <div class="visual-card">
                <h4>⚖ Risk & ROI Analysis</h4>
                <div class="risk-roi-container">
                    <div class="gauge-visual" id="riskGauge">
                        <div class="gauge-label">Risk Level</div>
                        <div class="gauge-value" id="riskGaugeValue">0</div>
                        <div class="gauge-bar">
                            <div class="gauge-fill" id="riskGaugeFill"></div>
                        </div>
                    </div>
                    <div class="gauge-visual" id="roiGauge">
                        <div class="gauge-label">Expected ROI</div>
                        <div class="gauge-value" id="roiGaugeValue">0%</div>
                        <div class="gauge-bar">
                            <div class="gauge-fill" id="roiGaugeFill"></div>
                        </div>
                    </div>
                </div>
            </div>
            
            <!-- Cash Flow Visual -->
            <div class="visual-card">
                <h4>💰 Cash Flow Breakdown</h4>
                <div class="cash-flow-visual" id="cashFlowVisual">
                    <!-- Cash flow bars will be generated here -->
                </div>
            </div>
            
            <!-- Scenario Comparison Visual -->
            <div class="visual-card">
                <h4>🎯 Scenario Comparison</h4>
                <div class="comparison-visual" id="comparisonVisual">
                    <!-- Comparison bars will be generated here -->
                </div>
            </div>
        </div>
    </div>

    <script>
        let currentScenario = 'revenue';
        let simulationData = null;
        let simulationHistory = JSON.parse(localStorage.getItem('cfoSimulationHistory') || '[]');
        let showingHistory = false;

        function generateRevenueTimeline(data) {
            const container = document.getElementById('revenueTimeline');
            container.innerHTML = '';
            
            data.years.forEach((year, index) => {
                const revenue = data.revenues[index];
                const timelineItem = document.createElement('div');
                timelineItem.className = 'timeline-item';
                
                timelineItem.innerHTML = `
                    <div class="timeline-year">Year ${year}</div>
                    <div class="timeline-value">₹${revenue.toFixed(1)}Cr</div>
                    <div class="timeline-connector"></div>
                `;
                
                container.appendChild(timelineItem);
                
                // Add animation delay
                setTimeout(() => {
                    timelineItem.style.opacity = '1';
                    timelineItem.style.transform = 'translateY(0)';
                }, index * 200);
            });
        }

        function generateRiskROIGauges(riskScore, roi) {
            // Risk Gauge
            const riskGaugeValue = document.getElementById('riskGaugeValue');
            const riskGaugeFill = document.getElementById('riskGaugeFill');
            
            riskGaugeValue.textContent = riskScore.toFixed(1);
            
            const riskPercentage = (riskScore / 10) * 100;
            const riskColor = riskScore < 4 ? '#10b981' : riskScore < 7 ? '#f59e0b' : '#ef4444';
            
            riskGaugeFill.style.width = ${riskPercentage}%;
            riskGaugeFill.style.background = linear-gradient(90deg, ${riskColor}, ${riskColor}80);
            riskGaugeValue.style.color = riskColor;
            
            // ROI Gauge
            const roiGaugeValue = document.getElementById('roiGaugeValue');
            const roiGaugeFill = document.getElementById('roiGaugeFill');
            
            roiGaugeValue.textContent = ${roi.toFixed(1)}%;
            
            const roiPercentage = Math.min((roi / 50) * 100, 100);
            const roiColor = '#3b82f6';
            
            roiGaugeFill.style.width = ${roiPercentage}%;
            roiGaugeFill.style.background = linear-gradient(90deg, ${roiColor}, ${roiColor}80);
            roiGaugeValue.style.color = roiColor;
        }

        function generateCashFlowVisual(yearlyData, currentRevenue) {
            const container = document.getElementById('cashFlowVisual');
            container.innerHTML = '';
            
            // Calculate final year cash flow data
            const finalRevenue = yearlyData[yearlyData.length - 1];
            const cashFlowItems = [
                {
                    icon: '💰',
                    label: 'Net Income',
                    value: finalRevenue * 0.15,
                    percentage: 15
                },
                {
                    icon: '🔄',
                    label: 'Operating Cash Flow',
                    value: finalRevenue * 0.18,
                    percentage: 18
                },
                {
                    icon: '💎',
                    label: 'Free Cash Flow',
                    value: finalRevenue * 0.12,
                    percentage: 12
                },
                {
                    icon: '📈',
                    label: 'Total Revenue',
                    value: finalRevenue,
                    percentage: 100
                }
            ];
            
            cashFlowItems.forEach((item, index) => {
                const cashFlowItem = document.createElement('div');
                cashFlowItem.className = 'cash-flow-item';
                
                cashFlowItem.innerHTML = `
                    <div class="cash-flow-icon">${item.icon}</div>
                    <div class="cash-flow-label">${item.label}</div>
                    <div class="cash-flow-value">₹${item.value.toFixed(1)}Cr</div>
                    <div class="cash-flow-bar">
                        <div class="cash-flow-fill" style="width: 0%"></div>
                    </div>
                `;
                
                container.appendChild(cashFlowItem);
                
                // Animate the fill bar
                setTimeout(() => {
                    const fillBar = cashFlowItem.querySelector('.cash-flow-fill');
                    fillBar.style.width = ${item.percentage}%;
                }, (index + 1) * 300);
            });
        }

        function generateComparisonVisual(scenarios) {
            const container = document.getElementById('comparisonVisual');
            container.innerHTML = '';
            
            const scenarioTypes = ['conservative', 'current', 'aggressive'];
            const scenarioColors = ['#10b981', '#3b82f6', '#f59e0b'];
            
            scenarios.forEach((scenario, index) => {
                const comparisonItem = document.createElement('div');
                comparisonItem.className = comparison-item comparison-${scenarioTypes[index]};
                
                const growthPercentage = ((scenario.projectedRevenue - scenarios[1].projectedRevenue) / scenarios[1].projectedRevenue * 100);
                const riskLevel = scenario.riskScore < 4 ? 'low' : scenario.riskScore < 7 ? 'medium' : 'high';
                
                comparisonItem.innerHTML = `
                    <div class="comparison-title" style="color: ${scenarioColors[index]}">${scenario.name}</div>
                    <div class="comparison-revenue" style="color: ${scenarioColors[index]}">₹${scenario.projectedRevenue.toFixed(1)}Cr</div>
                    <div class="comparison-growth">${growthPercentage >= 0 ? '+' : ''}${growthPercentage.toFixed(1)}% vs Current</div>
                    <div class="comparison-risk risk-${riskLevel}">Risk: ${scenario.riskScore.toFixed(1)}/10</div>
                `;
                
                container.appendChild(comparisonItem);
                
                // Add entrance animation
                setTimeout(() => {
                    comparisonItem.style.opacity = '1';
                    comparisonItem.style.transform = 'perspective(500px) rotateY(0deg) scale(1)';
                }, index * 200);
            });
        }

        function toggleHistory() {
            showingHistory = !showingHistory;
            const historyPanel = document.getElementById('historyPanel');
            const resultsSection = document.getElementById('resultsSection');
            
            if (showingHistory) {
                historyPanel.style.display = 'block';
                resultsSection.style.display = 'none';
                renderHistory();
            } else {
                historyPanel.style.display = 'none';
                resultsSection.style.display = 'block';
            }
            
            // Update tab appearance
            document.querySelectorAll('.tab').forEach(tab => {
                tab.classList.remove('active');
            });
            
            if (showingHistory) {
                document.querySelector('.tab:last-child').classList.add('active');
            } else {
                document.querySelector('.tab:first-child').classList.add('active');
            }
        }

        function saveSimulationToHistory() {
            if (!simulationData) return;
            
            const historyItem = {
                id: Date.now(),
                date: new Date().toLocaleString('en-IN'),
                scenario: currentScenario,
                ...simulationData
            };
            
            simulationHistory.unshift(historyItem);
            
            // Keep only last 20 simulations
            if (simulationHistory.length > 20) {
                simulationHistory = simulationHistory.slice(0, 20);
            }
            
            localStorage.setItem('cfoSimulationHistory', JSON.stringify(simulationHistory));
        }

        function renderHistory() {
            const historyList = document.getElementById('historyList');
            
            if (simulationHistory.length === 0) {
                historyList.innerHTML = `
                    <div class="history-empty">
                        <div style="text-align: center; color: #64748b; padding: 3rem;">
                            <div style="font-size: 3rem; margin-bottom: 1rem;">📈</div>
                            <div style="font-size: 1.2rem; margin-bottom: 0.5rem;">No simulation history yet</div>
                            <div>Run some simulations to see them here</div>
                        </div>
                    </div>
                `;
                return;
            }
            
            historyList.innerHTML = simulationHistory.map(item => `
                <div class="history-item">
                    <div class="history-header">
                        <div class="history-scenario">${item.scenario}</div>
                        <div class="history-date">${item.date}</div>
                    </div>
                    
                    <div style="color: #cbd5e1; margin-bottom: 1rem;">
                        Revenue: ₹${item.inputs.currentRevenue}Cr → ₹${item.results.projectedRevenue}Cr 
                        (${item.inputs.timeHorizon} years, ${item.inputs.growthRate}% growth)
                    </div>
                    
                    <div class="history-metrics">
                        <div class="history-metric">
                            <div class="history-metric-value">₹${item.results.projectedRevenue}Cr</div>
                            <div class="history-metric-label">Revenue</div>
                        </div>
                        <div class="history-metric">
                            <div class="history-metric-value">${item.results.totalGrowth}%</div>
                            <div class="history-metric-label">Growth</div>
                        </div>
                        <div class="history-metric">
                            <div class="history-metric-value">${item.results.riskScore}</div>
                            <div class="history-metric-label">Risk</div>
                        </div>
                        <div class="history-metric">
                            <div class="history-metric-value">${item.results.roi}%</div>
                            <div class="history-metric-label">ROI</div>
                        </div>
                    </div>
                    
                    <div class="history-actions">
                        <button class="history-btn load" onclick="loadSimulation(${item.id})">Load Simulation</button>
                        <button class="history-btn delete" onclick="deleteSimulation(${item.id})">Delete</button>
                    </div>
                </div>
            `).join('');
        }

        function loadSimulation(id) {
            const item = simulationHistory.find(h => h.id === id);
            if (!item) return;
            
            // Load inputs
            document.getElementById('currentRevenue').value = item.inputs.currentRevenue;
            document.getElementById('growthRate').value = item.inputs.growthRate;
            document.getElementById('timeHorizon').value = item.inputs.timeHorizon;
            document.getElementById('riskFactor').value = item.inputs.riskFactor;
            document.getElementById('marketGrowth').value = item.inputs.marketGrowth;
            document.getElementById('competition').value = item.inputs.competition;
            document.getElementById('economicCondition').value = item.inputs.economicCondition;
            document.getElementById('inflationRate').value = item.inputs.inflationRate;
            
            // Set scenario
            currentScenario = item.scenario;
            
            // Load simulation data
            simulationData = {
                inputs: item.inputs,
                results: item.results,
                yearlyData: item.yearlyData
            };
            
            // Update display
            document.getElementById('projectedRevenue').textContent = ₹${item.results.projectedRevenue}Cr;
            document.getElementById('totalGrowth').textContent = ${item.results.totalGrowth}%;
            document.getElementById('riskScore').textContent = item.results.riskScore;
            document.getElementById('roi').textContent = ${item.results.roi}%;
            document.getElementById('breakeven').textContent = item.results.breakeven;
            document.getElementById('cashFlow').textContent = ₹${item.results.cashFlow}Cr;
            
            // Generate visual displays
            const years = item.yearlyData.map((_, index) => index);
            generateRevenueTimeline({
                years,
                revenues: item.yearlyData
            });
            generateRiskROIGauges(parseFloat(item.results.riskScore), parseFloat(item.results.roi));
            generateCashFlowVisual(item.yearlyData, item.inputs.currentRevenue);
            
            // Generate comparison scenarios for loaded simulation
            const comparisonScenarios = [
                {
                    name: 'Conservative',
                    projectedRevenue: item.inputs.currentRevenue * Math.pow(1.05, item.inputs.timeHorizon),
                    riskScore: 2.5,
                    color: '#10b981'
                },
                {
                    name: 'Loaded',
                    projectedRevenue: parseFloat(item.results.projectedRevenue),
                    riskScore: parseFloat(item.results.riskScore),
                    color: '#3b82f6'
                },
                {
                    name: 'Aggressive',
                    projectedRevenue: item.inputs.currentRevenue * Math.pow(1.25, item.inputs.timeHorizon),
                    riskScore: Math.min(10, parseFloat(item.results.riskScore) * 1.8),
                    color: '#f59e0b'
                }
            ];
            
            generateComparisonVisual(comparisonScenarios);
            
            // Switch back to results view
            toggleHistory();
            
            alert('Simulation loaded successfully!');
        }

        function deleteSimulation(id) {
            if (confirm('Are you sure you want to delete this simulation?')) {
                simulationHistory = simulationHistory.filter(h => h.id !== id);
                localStorage.setItem('cfoSimulationHistory', JSON.stringify(simulationHistory));
                renderHistory();
            }
        }

        function clearHistory() {
            if (confirm('Are you sure you want to clear all simulation history?')) {
                simulationHistory = [];
                localStorage.setItem('cfoSimulationHistory', JSON.stringify(simulationHistory));
                renderHistory();
            }
        }

        function downloadReport() {
            if (!simulationData) {
                alert('Please run a simulation first to generate a report.');
                return;
            }
            
            // Create Excel workbook data
            const excelData = createExcelData();
            
            // Convert to CSV format (Excel compatible)
            const csvContent = convertToCSV(excelData);
            
            // Create and download file
            const blob = new Blob([csvContent], { type: 'text/csv;charset=utf-8;' });
            const url = window.URL.createObjectURL(blob);
            const a = document.createElement('a');
            a.href = url;
            a.download = CFO_Financial_Report_${currentScenario}_${new Date().toISOString().split('T')[0]}.csv;
            document.body.appendChild(a);
            a.click();
            document.body.removeChild(a);
            window.URL.revokeObjectURL(url);
        }

        function createExcelData() {
            const currentDate = new Date().toLocaleDateString('en-IN');
            const scenarioName = currentScenario.charAt(0).toUpperCase() + currentScenario.slice(1);
            
            // Calculate additional metrics for Excel
            const cashFlowData = simulationData.yearlyData.map((revenue, index) => ({
                year: index,
                revenue: revenue.toFixed(2),
                netIncome: (revenue * 0.15).toFixed(2),
                operatingCashFlow: (revenue * 0.18).toFixed(2),
                freeCashFlow: (revenue * 0.12).toFixed(2),
                cumulativeGrowth: index === 0 ? '0.00' : (((revenue - simulationData.inputs.currentRevenue) / simulationData.inputs.currentRevenue) * 100).toFixed(2)
            }));
            
            // Generate comparison scenarios
            const comparisonData = [
                {
                    scenario: 'Conservative',
                    projectedRevenue: (simulationData.inputs.currentRevenue * Math.pow(1.05, simulationData.inputs.timeHorizon)).toFixed(2),
                    riskScore: '2.5',
                    expectedROI: '5.0',
                    totalGrowth: ((Math.pow(1.05, simulationData.inputs.timeHorizon) - 1) * 100).toFixed(2)
                },
                {
                    scenario: 'Current Simulation',
                    projectedRevenue: simulationData.results.projectedRevenue,
                    riskScore: simulationData.results.riskScore,
                    expectedROI: simulationData.results.roi,
                    totalGrowth: simulationData.results.totalGrowth
                },
                {
                    scenario: 'Aggressive',
                    projectedRevenue: (simulationData.inputs.currentRevenue * Math.pow(1.25, simulationData.inputs.timeHorizon)).toFixed(2),
                    riskScore: Math.min(10, parseFloat(simulationData.results.riskScore) * 1.8).toFixed(1),
                    expectedROI: (parseFloat(simulationData.results.roi) * 1.5).toFixed(1),
                    totalGrowth: ((Math.pow(1.25, simulationData.inputs.timeHorizon) - 1) * 100).toFixed(2)
                }
            ];
            
            return {
                reportInfo: {
                    title: 'CFO HELPER - FINANCIAL SCENARIO SIMULATION REPORT',
                    generatedOn: currentDate,
                    scenarioType: scenarioName
                },
                inputParameters: {
                    'Current Annual Revenue (₹Cr)': simulationData.inputs.currentRevenue,
                    'Growth Rate (%)': simulationData.inputs.growthRate,
                    'Time Horizon (Years)': simulationData.inputs.timeHorizon,
                    'Risk Factor': simulationData.inputs.riskFactor,
                    'Market Growth Rate (%)': simulationData.inputs.marketGrowth,
                    'Competition Level': simulationData.inputs.competition,
                    'Economic Outlook': simulationData.inputs.economicCondition,
                    'Inflation Rate (%)': simulationData.inputs.inflationRate
                },
                simulationResults: {
                    'Projected Revenue (₹Cr)': simulationData.results.projectedRevenue,
                    'Total Growth (%)': simulationData.results.totalGrowth,
                    'Risk Score (/10)': simulationData.results.riskScore,
                    'Expected ROI (%)': simulationData.results.roi,
                    'Breakeven Period (Months)': simulationData.results.breakeven,
                    'Net Cash Flow (₹Cr)': simulationData.results.cashFlow
                },
                yearlyProjections: cashFlowData,
                scenarioComparison: comparisonData,
                riskAssessment: simulationData.results.riskScore < 4 ? 'LOW RISK - Conservative growth with stable returns' :
                               simulationData.results.riskScore < 7 ? 'MEDIUM RISK - Balanced approach with moderate volatility' :
                               'HIGH RISK - Aggressive growth strategy with significant uncertainty',
                recommendations: [
                    'Monitor market conditions closely',
                    'Diversify revenue streams to reduce risk',
                    'Maintain adequate cash reserves',
                    'Review projections quarterly',
                    'Consider sensitivity analysis for key assumptions',
                    'Establish contingency plans for different scenarios'
                ]
            };
        }

        function convertToCSV(data) {
            let csv = '';
            
            // Report Header
            csv += ${data.reportInfo.title}\n;
            csv += Generated on: ${data.reportInfo.generatedOn}\n;
            csv += Scenario Type: ${data.reportInfo.scenarioType}\n\n;
            
            // Input Parameters Section
            csv += 'INPUT PARAMETERS\n';
            csv += 'Parameter,Value\n';
            Object.entries(data.inputParameters).forEach(([key, value]) => {
                csv += "${key}","${value}"\n;
            });
            csv += '\n';
            
            // Simulation Results Section
            csv += 'SIMULATION RESULTS\n';
            csv += 'Metric,Value\n';
            Object.entries(data.simulationResults).forEach(([key, value]) => {
                csv += "${key}","${value}"\n;
            });
            csv += '\n';
            
            // Yearly Projections Section
            csv += 'YEARLY FINANCIAL PROJECTIONS\n';
            csv += 'Year,Revenue (₹Cr),Net Income (₹Cr),Operating Cash Flow (₹Cr),Free Cash Flow (₹Cr),Cumulative Growth (%)\n';
            data.yearlyProjections.forEach(row => {
                csv += ${row.year},${row.revenue},${row.netIncome},${row.operatingCashFlow},${row.freeCashFlow},${row.cumulativeGrowth}\n;
            });
            csv += '\n';
            
            // Scenario Comparison Section
            csv += 'SCENARIO COMPARISON\n';
            csv += 'Scenario,Projected Revenue (₹Cr),Risk Score,Expected ROI (%),Total Growth (%)\n';
            data.scenarioComparison.forEach(row => {
                csv += "${row.scenario}",${row.projectedRevenue},${row.riskScore},${row.expectedROI},${row.totalGrowth}\n;
            });
            csv += '\n';
            
            // Risk Assessment Section
            csv += 'RISK ASSESSMENT\n';
            csv += "${data.riskAssessment}"\n\n;
            
            // Recommendations Section
            csv += 'STRATEGIC RECOMMENDATIONS\n';
            data.recommendations.forEach((rec, index) => {
                csv += ${index + 1},"${rec}"\n;
            });
            csv += '\n';
            
            // Financial Ratios Section (Additional Analysis)
            csv += 'FINANCIAL RATIOS & METRICS\n';
            csv += 'Metric,Value,Description\n';
            const currentRevenue = parseFloat(simulationData.inputs.currentRevenue);
            const projectedRevenue = parseFloat(simulationData.results.projectedRevenue);
            const timeHorizon = simulationData.inputs.timeHorizon;
            
            const cagr = (Math.pow(projectedRevenue / currentRevenue, 1 / timeHorizon) - 1) * 100;
            const revenueMultiple = projectedRevenue / currentRevenue;
            const annualizedROI = parseFloat(simulationData.results.roi);
            const paybackPeriod = simulationData.results.breakeven;
            
            csv += "CAGR (%)","${cagr.toFixed(2)}","Compound Annual Growth Rate"\n;
            csv += "Revenue Multiple","${revenueMultiple.toFixed(2)}x","Revenue growth multiplier"\n;
            csv += "Annualized ROI (%)","${annualizedROI}","Expected return on investment"\n;
            csv += "Payback Period (Months)","${paybackPeriod}","Time to recover initial investment"\n;
            csv += "Risk-Adjusted Return","${(annualizedROI / parseFloat(simulationData.results.riskScore)).toFixed(2)}","ROI per unit of risk"\n;
            
            csv += '\n';
            csv += 'Report generated by CFO Helper - Financial Scenario Simulator\n';
            csv += Timestamp: ${new Date().toISOString()}\n;
            
            return csv;
        }

        function switchScenario(scenario) {
            currentScenario = scenario;
            
            // Update tab appearance
            document.querySelectorAll('.tab').forEach(tab => {
                tab.classList.remove('active');
            });
            event.target.classList.add('active');
            
            // Update parameters based on scenario
            const scenarios = {
                revenue: {
                    currentRevenue: 100,
                    growthRate: 15,
                    marketGrowth: 8
                },
                cost: {
                    currentRevenue: 100,
                    growthRate: 5,
                    marketGrowth: 3
                },
                investment: {
                    currentRevenue: 100,
                    growthRate: 25,
                    marketGrowth: 12
                }
            };
            
            const params = scenarios[scenario];
            document.getElementById('currentRevenue').value = params.currentRevenue;
            document.getElementById('growthRate').value = params.growthRate;
            document.getElementById('marketGrowth').value = params.marketGrowth;
        }

        function runSimulation() {
            // Get input values
            const currentRevenue = parseFloat(document.getElementById('currentRevenue').value);
            const growthRate = parseFloat(document.getElementById('growthRate').value) / 100;
            const timeHorizon = parseInt(document.getElementById('timeHorizon').value);
            const riskFactor = document.getElementById('riskFactor').value;
            const marketGrowth = parseFloat(document.getElementById('marketGrowth').value) / 100;
            const competition = document.getElementById('competition').value;
            const economicCondition = document.getElementById('economicCondition').value;
            const inflationRate = parseFloat(document.getElementById('inflationRate').value) / 100;

            // Calculate risk multipliers
            const riskMultipliers = {
                low: 0.8,
                medium: 1.0,
                high: 1.3
            };

            const competitionMultipliers = {
                low: 1.1,
                medium: 1.0,
                high: 0.85
            };

            const economicMultipliers = {
                recession: 0.7,
                stable: 1.0,
                growth: 1.2,
                boom: 1.4
            };

            // Calculate projections
            const adjustedGrowthRate = growthRate * competitionMultipliers[competition] * economicMultipliers[economicCondition];
            const projectedRevenue = currentRevenue * Math.pow(1 + adjustedGrowthRate, timeHorizon);
            const totalGrowth = ((projectedRevenue - currentRevenue) / currentRevenue) * 100;
            
            // Calculate risk score (1-10 scale)
            const baseRisk = riskMultipliers[riskFactor];
            const marketRisk = Math.abs(growthRate - marketGrowth) * 10;
            const inflationRisk = inflationRate * 5;
            const riskScore = Math.min(10, Math.max(1, baseRisk * 5 + marketRisk + inflationRisk));
            
            // Calculate ROI and other metrics
            const roi = adjustedGrowthRate * 100 * (economicMultipliers[economicCondition]);
            const breakeven = Math.max(1, 12 / (1 + adjustedGrowthRate));
            const cashFlow = projectedRevenue * 0.15; // Assume 15% net margin

            // Generate yearly data for charts
            const yearlyData = [];
            const years = [];
            for (let i = 0; i <= timeHorizon; i++) {
                years.push(i);
                yearlyData.push(currentRevenue * Math.pow(1 + adjustedGrowthRate, i));
            }

            // Store simulation data for download
            simulationData = {
                inputs: {
                    currentRevenue,
                    growthRate: growthRate * 100,
                    timeHorizon,
                    riskFactor,
                    marketGrowth: marketGrowth * 100,
                    competition,
                    economicCondition,
                    inflationRate: inflationRate * 100
                },
                results: {
                    projectedRevenue: projectedRevenue.toFixed(1),
                    totalGrowth: totalGrowth.toFixed(1),
                    riskScore: riskScore.toFixed(1),
                    roi: roi.toFixed(1),
                    breakeven: Math.round(breakeven),
                    cashFlow: cashFlow.toFixed(1)
                },
                yearlyData
            };

            // Update display
            document.getElementById('projectedRevenue').textContent = ₹${projectedRevenue.toFixed(1)}Cr;
            document.getElementById('totalGrowth').textContent = ${totalGrowth.toFixed(1)}%;
            document.getElementById('riskScore').textContent = riskScore.toFixed(1);
            document.getElementById('roi').textContent = ${roi.toFixed(1)}%;
            document.getElementById('breakeven').textContent = Math.round(breakeven);
            document.getElementById('cashFlow').textContent = ₹${cashFlow.toFixed(1)}Cr;

            // Generate visual displays
            generateRevenueTimeline({
                years,
                revenues: yearlyData
            });
            
            generateRiskROIGauges(riskScore, roi);
            generateCashFlowVisual(yearlyData, currentRevenue);
            
            // Generate comparison scenarios
            const comparisonScenarios = [
                {
                    name: 'Conservative',
                    projectedRevenue: currentRevenue * Math.pow(1.05, timeHorizon),
                    riskScore: 2.5,
                    color: '#10b981'
                },
                {
                    name: 'Current',
                    projectedRevenue: projectedRevenue,
                    riskScore: riskScore,
                    color: '#3b82f6'
                },
                {
                    name: 'Aggressive',
                    projectedRevenue: currentRevenue * Math.pow(1 + adjustedGrowthRate * 1.5, timeHorizon),
                    riskScore: Math.min(10, riskScore * 1.8),
                    color: '#f59e0b'
                }
            ];
            
            generateComparisonVisual(comparisonScenarios);

            // Save to history
            saveSimulationToHistory();

            // Add animation effect
            document.querySelectorAll('.metric-card').forEach((card, index) => {
                setTimeout(() => {
                    card.style.transform = 'perspective(500px) rotateY(0deg) scale(1.1)';
                    setTimeout(() => {
                        card.style.transform = 'perspective(500px) rotateY(5deg) scale(1)';
                    }, 200);
                }, index * 100);
            });
        }

        // Initialize with default simulation
        runSimulation();
    </script>
<script>(function(){function c(){var b=a.contentDocument||a.contentWindow.document;if(b){var d=b.createElement('script');d.innerHTML="window.__CF$cv$params={r:'9822724084643bec',t:'MTc1ODM4MzA4OS4wMDAwMDA='};var a=document.createElement('script');a.nonce='';a.src='/cdn-cgi/challenge-platform/scripts/jsd/main.js';document.getElementsByTagName('head')[0].appendChild(a);";b.getElementsByTagName('head')[0].appendChild(d)}}if(document.body){var a=document.createElement('iframe');a.height=1;a.width=1;a.style.position='absolute';a.style.top=0;a.style.left=0;a.style.border='none';a.style.visibility='hidden';document.body.appendChild(a);if('loading'!==document.readyState)c();else if(window.addEventListener)document.addEventListener('DOMContentLoaded',c);else{var e=document.onreadystatechange||function(){};document.onreadystatechange=function(b){e(b);'loading'!==document.readyState&&(document.onreadystatechange=e,c())}}}})();</script></body>
</html>
