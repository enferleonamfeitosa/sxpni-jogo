# sxpni-jogo
Aqui está a versão completamente aprimorada do VAXNET_OS // SI-PNI.
O design foi totalmente reformulado para um layout limpo, espaçoso e responsivo, reposicionando o mascote Byte para o canto superior direito em uma barra flutuante dedicada, garantindo que ele nunca cubra botões, formulários ou controles. Além disso, o foco em estudantes iniciantes foi fortemente expandido: agora o jogo traz dicas clínicas explícitas e detalhadas em todas as missões, informando com clareza o lado anatômico correto (ex: Músculo Vasto Lateral da Coxa Esquerda/Direita, Músculo Deltoide Direito/Esquerdo) e justificativas passo a passo conforme os Manuais do Ministério da Saúde.
Salve o código abaixo como um arquivo .html (ex: sxpni_jogo.html) e abra em qualquer navegador offline.
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>VAXNET_OS // SI-PNI - Treinamento de Sala de Vacina</title>
    <!-- Google Fonts: Orbitron, Rajdhani, VT323 com fallback -->
    <link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@500;700;900&family=Rajdhani:wght@500;600;700&family=VT323&display=swap" rel="stylesheet">
    <style>
        :root {
            --bg-deep: #050814;
            --panel-bg: rgba(10, 15, 30, 0.88);
            --border-glow: #00f0ff;
            --neon-cyan: #00f0ff;
            --neon-pink: #ff2a6d;
            --neon-yellow: #fcee09;
            --text-main: #e0fbfc;
            --text-dim: #8892b0;
            --success: #05ffa1;
            --error: #ff3366;
            --font-title: 'Orbitron', sans-serif;
            --font-ui: 'Rajdhani', sans-serif;
            --font-terminal: 'VT323', monospace;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            user-select: none;
        }

        body {
            background-color: var(--bg-deep);
            background-image: 
                linear-gradient(rgba(0, 240, 255, 0.03) 1px, transparent 1px),
                linear-gradient(90deg, rgba(0, 240, 255, 0.03) 1px, transparent 1px);
            background-size: 30px 30px;
            color: var(--text-main);
            font-family: var(--font-ui);
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            overflow-x: hidden;
        }

        /* Top Bar & Audio Controls */
        header {
            background: rgba(5, 8, 20, 0.95);
            border-bottom: 2px solid var(--neon-cyan);
            padding: 10px 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            position: sticky;
            top: 0;
            z-index: 1000;
            box-shadow: 0 0 15px rgba(0, 240, 255, 0.2);
        }

        .logo-area {
            display: flex;
            align-items: center;
            gap: 12px;
        }

        .logo-icon {
            font-family: var(--font-title);
            font-size: 1.4rem;
            color: var(--neon-cyan);
            text-shadow: 0 0 8px var(--neon-cyan);
        }

        .logo-text {
            font-family: var(--font-title);
            font-size: 1.1rem;
            letter-spacing: 1px;
            color: var(--text-main);
        }

        .audio-controls {
            display: flex;
            gap: 10px;
            align-items: center;
        }

        .btn-audio {
            background: rgba(0, 240, 255, 0.1);
            border: 1px solid var(--neon-cyan);
            color: var(--neon-cyan);
            padding: 6px 12px;
            font-family: var(--font-ui);
            font-weight: 700;
            cursor: pointer;
            border-radius: 4px;
            transition: all 0.2s;
            font-size: 0.9rem;
        }

        .btn-audio:hover {
            background: var(--neon-cyan);
            color: var(--bg-deep);
            box-shadow: 0 0 10px var(--neon-cyan);
        }

        .btn-audio.muted {
            border-color: var(--text-dim);
            color: var(--text-dim);
            background: transparent;
        }

        /* Main Container */
        .main-container {
            flex: 1;
            max-width: 1200px;
            width: 100%;
            margin: 0 auto;
            padding: 20px;
            display: flex;
            flex-direction: column;
            gap: 20px;
            position: relative;
        }

        /* Mascot Header Bar (Fixed & Non-Obtrusive) */
        .mascot-bar {
            background: rgba(15, 23, 42, 0.9);
            border: 1px solid var(--neon-pink);
            border-radius: 8px;
            padding: 12px 18px;
            display: flex;
            align-items: center;
            gap: 15px;
            box-shadow: 0 0 15px rgba(255, 42, 109, 0.2);
        }

        .mascot-avatar {
            width: 45px;
            height: 45px;
            background: var(--neon-pink);
            border-radius: 50%;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            position: relative;
            box-shadow: 0 0 10px var(--neon-pink);
            animation: floatMascot 3s ease-in-out infinite;
            flex-shrink: 0;
        }

        .mascot-visor {
            width: 26px;
            height: 10px;
            background: var(--neon-cyan);
            border-radius: 3px;
            animation: blinkVisor 4s infinite;
        }

        .mascot-smile {
            width: 14px;
            height: 4px;
            border-bottom: 2px solid var(--bg-deep);
            border-radius: 0 0 8px 8px;
            margin-top: 3px;
        }

        @keyframes floatMascot {
            0%, 100% { transform: translateY(0); }
            50% { transform: translateY(-4px); }
        }

        @keyframes blinkVisor {
            0%, 90%, 100% { transform: scaleY(1); }
            95% { transform: scaleY(0.1); }
        }

        .mascot-dialogue {
            font-size: 1.05rem;
            color: var(--text-main);
            line-height: 1.4;
            flex: 1;
        }

        .mascot-dialogue strong {
            color: var(--neon-yellow);
            font-family: var(--font-title);
            font-size: 0.9rem;
            display: inline-block;
            margin-right: 5px;
        }

        /* Screens */
        .screen {
            display: none;
            flex-direction: column;
            gap: 20px;
            animation: fadeIn 0.3s ease-in-out;
        }

        .screen.active {
            display: flex;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(6px); }
            to { opacity: 1; transform: translateY(0); }
        }

        /* Panels & Cards */
        .panel {
            background: var(--panel-bg);
            border: 1px solid var(--neon-cyan);
            border-radius: 6px;
            padding: 20px;
            position: relative;
            box-shadow: 0 0 15px rgba(0, 240, 255, 0.1);
            clip-path: polygon(0 0, calc(100% - 15px) 0, 100% 15px, 100% 100%, 15px 100%, 0 calc(100% - 15px));
        }

        .panel-title {
            font-family: var(--font-title);
            color: var(--neon-cyan);
            font-size: 1.2rem;
            margin-bottom: 15px;
            display: flex;
            align-items: center;
            gap: 10px;
            text-shadow: 0 0 5px rgba(0, 240, 255, 0.4);
        }

        /* Buttons */
        .btn {
            background: linear-gradient(135deg, rgba(0, 240, 255, 0.2), rgba(0, 240, 255, 0.05));
            border: 2px solid var(--neon-cyan);
            color: var(--neon-cyan);
            padding: 12px 24px;
            font-family: var(--font-title);
            font-weight: 700;
            font-size: 1rem;
            cursor: pointer;
            border-radius: 4px;
            transition: all 0.2s;
            text-transform: uppercase;
            letter-spacing: 1px;
            box-shadow: 0 0 10px rgba(0, 240, 255, 0.2);
        }

        .btn:hover {
            background: var(--neon-cyan);
            color: var(--bg-deep);
            box-shadow: 0 0 20px var(--neon-cyan);
            transform: translateY(-2px);
        }

        .btn-pink {
            border-color: var(--neon-pink);
            color: var(--neon-pink);
            background: linear-gradient(135deg, rgba(255, 42, 109, 0.2), rgba(255, 42, 109, 0.05));
        }

        .btn-pink:hover {
            background: var(--neon-pink);
            color: var(--bg-deep);
            box-shadow: 0 0 20px var(--neon-pink);
        }

        .btn-yellow {
            border-color: var(--neon-yellow);
            color: var(--neon-yellow);
            background: linear-gradient(135deg, rgba(252, 238, 9, 0.2), rgba(252, 238, 9, 0.05));
        }

        .btn-yellow:hover {
            background: var(--neon-yellow);
            color: var(--bg-deep);
            box-shadow: 0 0 20px var(--neon-yellow);
        }

        /* Welcome Screen */
        .hero-banner {
            text-align: center;
            padding: 30px 20px;
            display: flex;
            flex-direction: column;
            gap: 15px;
        }

        .hero-title {
            font-family: var(--font-title);
            font-size: 2.5rem;
            color: var(--neon-cyan);
            text-shadow: 0 0 15px rgba(0, 240, 255, 0.6);
        }

        .hero-subtitle {
            font-size: 1.2rem;
            color: var(--text-dim);
            max-width: 750px;
            margin: 0 auto;
            line-height: 1.5;
        }

        .steps-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 15px;
            margin-top: 10px;
        }

        .step-card {
            background: rgba(10, 15, 30, 0.9);
            border: 1px solid var(--border-glow);
            padding: 15px;
            border-radius: 6px;
            text-align: left;
            display: flex;
            flex-direction: column;
            gap: 8px;
        }

        .step-number {
            font-family: var(--font-title);
            color: var(--neon-yellow);
            font-size: 1.1rem;
        }

        .step-card h4 {
            font-size: 1.05rem;
            color: var(--neon-cyan);
        }

        .step-card p {
            font-size: 0.9rem;
            color: var(--text-dim);
            line-height: 1.3;
        }

        .action-row {
            display: flex;
            justify-content: center;
            gap: 20px;
            margin-top: 20px;
        }

        /* HUD & Stats Bar */
        .hud-bar {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 15px;
            background: rgba(10, 15, 30, 0.95);
            border: 1px solid var(--neon-cyan);
            padding: 15px;
            border-radius: 6px;
        }

        .hud-item {
            display: flex;
            flex-direction: column;
            gap: 5px;
        }

        .hud-label {
            font-size: 0.85rem;
            color: var(--text-dim);
            font-family: var(--font-title);
            text-transform: uppercase;
        }

        .hud-value {
            font-family: var(--font-title);
            font-size: 1.2rem;
            color: var(--neon-cyan);
        }

        .progress-bar-container {
            width: 100%;
            height: 10px;
            background: rgba(255, 255, 255, 0.1);
            border-radius: 5px;
            overflow: hidden;
            border: 1px solid var(--neon-cyan);
        }

        .progress-bar-fill {
            height: 100%;
            background: var(--success);
            width: 100%;
            transition: width 0.3s ease;
        }

        /* Terminal Simulation Screen */
        .terminal-wrapper {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 20px;
        }

        @media (max-width: 900px) {
            .terminal-wrapper {
                grid-template-columns: 1fr;
            }
        }

        .terminal-box {
            background: #020408;
            border: 1px solid var(--neon-cyan);
            border-radius: 6px;
            padding: 15px;
            font-family: var(--font-terminal);
            font-size: 1.3rem;
            color: var(--success);
            min-height: 160px;
            max-height: 220px;
            overflow-y: auto;
            display: flex;
            flex-direction: column;
            gap: 5px;
            box-shadow: inset 0 0 10px rgba(0, 240, 255, 0.2);
        }

        .terminal-prompt-line {
            display: flex;
            gap: 10px;
            align-items: center;
            margin-top: 10px;
        }

        .terminal-input {
            background: transparent;
            border: none;
            border-bottom: 2px solid var(--success);
            color: var(--success);
            font-family: var(--font-terminal);
            font-size: 1.3rem;
            outline: none;
            flex: 1;
            padding: 2px 5px;
        }

        /* Clinical Guide Panel */
        .guide-panel {
            background: rgba(20, 15, 35, 0.9);
            border: 1px solid var(--neon-yellow);
            border-radius: 6px;
            padding: 15px;
            display: flex;
            flex-direction: column;
            gap: 10px;
        }

        .guide-title {
            font-family: var(--font-title);
            color: var(--neon-yellow);
            font-size: 1.05rem;
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .guide-content {
            font-size: 1rem;
            color: var(--text-main);
            line-height: 1.4;
        }

        .guide-content ul {
            padding-left: 20px;
            margin-top: 5px;
        }

        .guide-content li {
            margin-bottom: 3px;
        }

        /* Patient & Form Layout */
        .patient-card {
            background: rgba(10, 20, 35, 0.8);
            border: 1px solid var(--neon-cyan);
            border-radius: 6px;
            padding: 15px;
            display: flex;
            flex-direction: column;
            gap: 10px;
        }

        .patient-info-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(140px, 1fr));
            gap: 10px;
            font-size: 0.95rem;
        }

        .patient-info-item span {
            color: var(--text-dim);
            display: block;
            font-size: 0.8rem;
            text-transform: uppercase;
        }

        .patient-info-item strong {
            color: var(--neon-cyan);
            font-size: 1.05rem;
        }

        .history-table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 5px;
            font-size: 0.9rem;
        }

        .history-table th, .history-table td {
            border: 1px solid rgba(0, 240, 255, 0.3);
            padding: 6px 10px;
            text-align: left;
        }

        .history-table th {
            background: rgba(0, 240, 255, 0.1);
            color: var(--neon-cyan);
            font-family: var(--font-title);
            font-size: 0.8rem;
        }

        /* Form Fields */
        .form-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
            gap: 15px;
            margin-top: 10px;
        }

        .form-group {
            display: flex;
            flex-direction: column;
            gap: 6px;
        }

        .form-group label {
            font-size: 0.9rem;
            font-family: var(--font-title);
            color: var(--neon-cyan);
        }

        .form-control {
            background: rgba(5, 10, 20, 0.9);
            border: 1px solid var(--neon-cyan);
            color: var(--text-main);
            padding: 10px;
            font-family: var(--font-ui);
            font-size: 1rem;
            border-radius: 4px;
            outline: none;
            transition: all 0.2s;
        }

        .form-control:focus {
            box-shadow: 0 0 10px var(--neon-cyan);
            border-color: var(--success);
        }

        .form-actions {
            display: flex;
            gap: 15px;
            margin-top: 20px;
            justify-content: flex-end;
        }

        /* Feedback Modal / Overlay */
        .modal-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(2, 4, 10, 0.85);
            display: none;
            justify-content: center;
            align-items: center;
            z-index: 2000;
            padding: 20px;
        }

        .modal-content {
            background: var(--panel-bg);
            border: 2px solid var(--neon-cyan);
            border-radius: 8px;
            padding: 30px;
            max-width: 600px;
            width: 100%;
            display: flex;
            flex-direction: column;
            gap: 20px;
            box-shadow: 0 0 30px rgba(0, 240, 255, 0.3);
            text-align: center;
        }

        .modal-title {
            font-family: var(--font-title);
            font-size: 1.8rem;
            color: var(--success);
        }

        .modal-title.error {
            color: var(--error);
            text-shadow: 0 0 10px var(--error);
        }

        .modal-body {
            font-size: 1.05rem;
            line-height: 1.5;
            color: var(--text-main);
            text-align: left;
            background: rgba(0, 0, 0, 0.4);
            padding: 15px;
            border-radius: 6px;
            border: 1px solid rgba(255, 255, 255, 0.1);
        }

        .stars-display {
            font-size: 2rem;
            color: var(--neon-yellow);
            letter-spacing: 5px;
        }

        /* Utility Helpers */
        .highlight-field {
            animation: pulseField 1.5s infinite;
            border-color: var(--neon-yellow) !important;
        }

        @keyframes pulseField {
            0% { box-shadow: 0 0 0 0 rgba(252, 238, 9, 0.6); }
            70% { box-shadow: 0 0 0 8px rgba(252, 238, 9, 0); }
            100% { box-shadow: 0 0 0 0 rgba(252, 238, 9, 0); }
        }

        .badge {
            display: inline-block;
            padding: 3px 8px;
            border-radius: 4px;
            font-size: 0.8rem;
            font-weight: 700;
            text-transform: uppercase;
        }

        .badge-success { background: rgba(5, 255, 161, 0.2); color: var(--success); border: 1px solid var(--success); }
        .badge-error { background: rgba(255, 51, 102, 0.2); color: var(--error); border: 1px solid var(--error); }
    </style>
</head>
<body>

    <!-- Header / Top Bar -->
    <header>
        <div class="logo-area">
            <span class="logo-icon">💉</span>
            <span class="logo-text">VAXNET_OS // SI-PNI</span>
        </div>
        <div class="audio-controls">
            <button id="btnMusicToggle" class="btn-audio" onclick="toggleMusic()">🎵 Música: OFF</button>
            <button id="btnSfxToggle" class="btn-audio" onclick="toggleSfx()">🔊 Efeitos: ON</button>
        </div>
    </header>

    <div class="main-container">

        <!-- Mascot Fixed Top Banner (Non-obstructive) -->
        <div class="mascot-bar">
            <div class="mascot-avatar">
                <div class="mascot-visor"></div>
                <div class="mascot-smile"></div>
            </div>
            <div class="mascot-dialogue" id="mascotText">
                <strong>Byte:</strong> Olá, Enfermeiro(a) Hacker! Bem-vindo ao Neo-Sampa 2077. Estou aqui para guiar seus primeiros passos na sala de vacina! Clique em <strong>Iniciar Treinamento</strong> para começar.
            </div>
        </div>

        <!-- SCREEN 1: WELCOME & EDUCATIONAL HOME -->
        <section id="screenWelcome" class="screen active">
            <div class="panel hero-banner">
                <h1 class="hero-title">SIMULADOR DE SALA DE VACINA</h1>
                <p class="hero-subtitle">
                    Domine o <strong>SI-PNI</strong> (Sistema de Informação do Programa Nacional de Imunizações). Aprenda na prática a registrar doses, consultar históricos, gerenciar lotes e orientar pacientes com segurança absoluta.
                </p>

                <div class="steps-grid">
                    <div class="step-card">
                        <span class="step-number">PASSO 01</span>
                        <h4>Acesso ao Terminal</h4>
                        <p>Faça login simulado no sistema credenciado da Rede de Saúde.</p>
                    </div>
                    <div class="step-card">
                        <span class="step-number">PASSO 02</span>
                        <h4>Busca de Cidadão</h4>
                        <p>Localize o paciente por CPF ou CNS no banco de dados da cidade.</p>
                    </div>
                    <div class="step-card">
                        <span class="step-number">PASSO 03</span>
                        <h4>Análise Clínica</h4>
                        <p>Confira dados, histórico prévio, intervalos e contraindicações.</p>
                    </div>
                    <div class="step-card">
                        <span class="step-number">PASSO 04</span>
                        <h4>Registro de Dose</h4>
                        <p>Selecione vacina, dose, lote válido, via, <strong>lado anatômico</strong> e estratégia.</p>
                    </div>
                </div>

                <div class="action-row">
                    <button class="btn" onclick="startGame()">🚀 Iniciar Treinamento</button>
                    <button class="btn btn-pink" onclick="showMiniTutorial()">💡 Ver Como Funciona</button>
                </div>
            </div>
        </section>

        <!-- SCREEN 2: MISSION / SIMULATION SCREEN -->
        <section id="screenMission" class="screen">
            <!-- HUD & Stats -->
            <div class="hud-bar">
                <div class="hud-item">
                    <span class="hud-label">Missão Atual</span>
                    <span class="hud-value" id="hudMissionNum">01 / 10</span>
                </div>
                <div class="hud-item">
                    <span class="hud-label">Integridade (HP)</span>
                    <div class="progress-bar-container">
                        <div class="progress-bar-fill" id="hpBar" style="width: 100%;"></div>
                    </div>
                </div>
                <div class="hud-item">
                    <span class="hud-label">Reputação (XP)</span>
                    <span class="hud-value" id="hudXp">0 PTS</span>
                </div>
                <div class="hud-item" style="text-align: right;">
                    <button class="btn btn-yellow" style="padding: 6px 12px; font-size: 0.85rem;" onclick="triggerHint()">💡 Dica do Byte</button>
                </div>
            </div>

            <!-- Terminal Login / Search Sub-Screen -->
            <div id="subScreenLogin" class="panel">
                <h3 class="panel-title">💻 terminal.vaxnet // login_auth</h3>
                <p style="margin-bottom: 15px; color: var(--text-dim);">Insira suas credenciais corporativas da Secretaria de Saúde de Neo-Sampa para acessar o módulo SI-PNI.</p>
                <div class="form-grid">
                    <div class="form-group">
                        <label>Usuário SI-PNI</label>
                        <input type="text" id="loginUser" class="form-control" value="enf.hacker01">
                    </div>
                    <div class="form-group">
                        <label>Senha de Acesso</label>
                        <input type="password" id="loginPass" class="form-control" value="••••••••">
                    </div>
                </div>
                <div class="form-actions">
                    <button class="btn" onclick="processLogin()">Autenticar Acesso</button>
                </div>
            </div>

            <div id="subScreenSearch" class="panel" style="display: none;">
                <h3 class="panel-title">🔍 terminal.vaxnet // consulta_cidadao</h3>
                <p style="margin-bottom: 15px; color: var(--text-dim);">Pesquise o cidadão no banco de dados municipal utilizando o CPF ou Cartão Nacional de Saúde (CNS).</p>
                <div style="display: flex; gap: 10px; align-items: flex-end;">
                    <div class="form-group" style="flex: 1;">
                        <label>CPF ou CNS do Cidadão</label>
                        <input type="text" id="searchInput" class="form-control" placeholder="Digite ou clique em preencher automático...">
                    </div>
                    <button class="btn btn-yellow" id="btnAutoCpf" onclick="fillCpf()">⚡ Preencher CPF</button>
                    <button class="btn" onclick="executeSearch()">Buscar Paciente</button>
                </div>
            </div>

            <!-- Clinical Guide Panel & Patient Sheet (Active during vaccination) -->
            <div id="subScreenWorkspace" style="display: none; display: grid; grid-template-columns: 1fr; gap: 20px;">
                
                <!-- Clinical Guide for Beginner Support -->
                <div class="guide-panel" id="guidePanelContainer">
                    <div class="guide-title">🎯 GUIA VACINAL DO MINISTÉRIO DA SAÚDE (ORIENTAÇÃO PARA INICIANTES)</div>
                    <div class="guide-content" id="guideContentText">
                        <!-- Dynamic guide info populated via JS -->
                    </div>
                </div>

                <!-- Patient Sheet Panel -->
                <div class="panel">
                    <h3 class="panel-title">📋 Prontuário e Histórico Vacinal do Cidadão</h3>
                    <div class="patient-card">
                        <div class="patient-info-grid">
                            <div class="patient-info-item">
                                <span>Nome Completo</span>
                                <strong id="ptName">-</strong>
                            </div>
                            <div class="patient-info-item">
                                <span>Data de Nascimento</span>
                                <strong id="ptDob">-</strong>
                            </div>
                            <div class="patient-info-item">
                                <span>Idade Atual</span>
                                <strong id="ptAge">-</strong>
                            </div>
                            <div class="patient-info-item">
                                <span>Condição / Grupo</span>
                                <strong id="ptCondition">-</strong>
                            </div>
                        </div>
                        <div style="margin-top: 10px;">
                            <span style="font-size: 0.85rem; color: var(--text-dim); text-transform: uppercase; font-family: var::font-title;">Histórico de Doses Registradas no SI-PNI:</span>
                            <table class="history-table">
                               <thead>
                                   <tr>
                                       <th>Vacina</th>
                                       <th>Dose</th>
                                       <th>Data</th>
                                       <th>Lote</th>
                                   </tr>
                               </thead>
                               <tbody id="ptHistoryBody">
                                   <!-- Populated dynamically -->
                               </tbody>
                            </table>
                        </div>
                    </div>
                </div>

                <!-- Vaccination Form Panel -->
                <div class="panel">
                    <h3 class="panel-title">💉 Formulário de Registro de Imunobiológico (SI-PNI)</h3>
                    <form id="vaxForm" onsubmit="submitVaccineForm(event)">
                        <div class="form-grid">
                            <div class="form-group">
                                <label>Decisão Clínica</label>
                                <select id="fieldAction" class="form-control" onchange="toggleActionFields()">
                                    <option value="vaccinate">Administrar Imunobiológico</option>
                                    <option value="contraindication">Registrar Contraindicação / Não Vacinar</option>
                                </select>
                            </div>
                            <div class="form-group" id="groupVaccine">
                                <label>Imunobiológico / Vacina</label>
                                <select id="fieldVaccine" class="form-control">
                                    <option value="">-- Selecione a Vacina --</option>
                                </select>
                            </div>
                            <div class="form-group" id="groupDose">
                                <label>Dose Aplicada</label>
                                <select id="fieldDose" class="form-control">
                                    <option value="">-- Selecione a Dose --</option>
                                    <option value="D1">1ª Dose (D1)</option>
                                    <option value="D2">2ª Dose (D2)</option>
                                    <option value="D3">3ª Dose (D3)</option>
                                    <option value="Reforço">1º Reforço</option>
                                    <option value="2º Reforço">2º Reforço</option>
                                    <option value="Dose Única">Dose Única (DU)</option>
                                    <option value="Anual">Dose Anual / Campanha</option>
                                </select>
                            </div>
                            <div class="form-group" id="groupBatch">
                                <label>Número do Lote</label>
                                <select id="fieldBatch" class="form-control">
                                    <option value="">-- Selecione o Lote --</option>
                                </select>
                            </div>
                            <div class="form-group" id="groupRoute">
                                <label>Via de Administração</label>
                                <select id="fieldRoute" class="form-control">
                                    <option value="">-- Selecione a Via --</option>
                                    <option value="Intramuscular">Intramuscular (IM)</option>
                                    <option value="Subcutânea">Subcutânea (SC)</option>
                                    <option value="Intradérmica">Intradérmica (ID)</option>
                                    <option value="Oral">Oral (VO)</option>
                                </select>
                            </div>
                            <div class="form-group" id="groupAnatomic">
                                <label>Lado / Local Anatômico</label>
                                <select id="fieldAnatomic" class="form-control">
                                    <option value="">-- Selecione o Local Anatômico --</option>
                                    <option value="Deltoide Direito">Músculo Deltoide Direito</option>
                                    <option value="Deltoide Esquerdo">Músculo Deltoide Esquerdo</option>
                                    <option value="Vasto Lateral Coxa Direita">Músculo Vasto Lateral da Coxa Direita</option>
                                    <option value="Vasto Lateral Coxa Esquerda">Músculo Vasto Lateral da Coxa Esquerda</option>
                                    <option value="Via Oral">Cavidade Oral (Gotas)</option>
                                </select>
                            </div>
                            <div class="form-group" id="groupStrategy">
                                <label>Estratégia de Vacinação</label>
                                <select id="fieldStrategy" class="form-control">
                                    <option value="Rotina">Rotina (UBS)</option>
                                    <option value="Campanha">Campanha Nacional</option>
                                    <option value="Bloqueio">Bloqueio Vacinal</option>
                                </select>
                            </div>
                            <div class="form-group" id="groupReason" style="display:none;">
                                <label>Motivo de Não Vacinação / Adiamento</label>
                                <select id="fieldReason" class="form-control">
                                    <option value="">-- Selecione o Motivo --</option>
                                    <option value="Lote Vencido">Lote Vencido / Impróprio</option>
                                    <option value="Intervalo Insuficiente">Intervalo Mínimo não Respeitado</option>
                                    <option value="Contraindicacao Clinica">Contraindicação Clínica / Doença Aguda</option>
                                    <option value="Esquema Completo">Esquema Já Completo</option>
                                </select>
                            </div>
                        </div>
                        <div class="form-actions">
                            <button type="submit" class="btn">💾 Salvar Registro no SI-PNI</button>
                        </div>
                    </form>
                </div>

            </div>
        </section>

        <!-- SCREEN 3: FINAL GAME SUMMARY -->
        <section id="screenSummary" class="screen">
            <div class="panel" style="text-align: center; padding: 40px 20px;">
                <h2 class="hero-title" style="font-size: 2rem;">TREINAMENTO SI-PNI CONCLUÍDO!</h2>
                <p class="hero-subtitle" id="summaryFinalText">Você passou por todos os desafios da sala de vacina em Neo-Sampa.</p>
                
                <div style="display: flex; justify-content: center; gap: 30px; margin: 30px 0;">
                    <div class="hud-item">
                        <span class="hud-label">Pontuação Final</span>
                        <span class="hud-value" id="summaryScore" style="font-size: 1.8rem;">0</span>
                    </div>
                    <div class="hud-item">
                        <span class="hud-label">Missões Cumpridas</span>
                        <span class="hud-value" id="summaryCompleted" style="font-size: 1.8rem;">10/10</span>
                    </div>
                </div>

                <div class="action-row">
                    <button class="btn" onclick="restartGame()">🔄 Jogar Novamente</button>
                </div>
            </div>
        </section>

    </div>

    <!-- FEEDBACK MODAL -->
    <div id="feedbackModal" class="modal-overlay">
        <div class="modal-content">
            <h3 id="modalTitle" class="modal-title">RESULTADO</h3>
            <div id="modalBody" class="modal-body">
                <!-- Detailed explanation -->
            </div>
            <div class="action-row" style="margin-top: 0;">
                <button class="btn" id="modalNextBtn" onclick="closeModalAndProceed()">Continuar Treinamento</button>
            </div>
        </div>
    </div>

    <!-- AUDIO & GAME LOGIC SCRIPT -->
    <script>
        /* ==========================================
           1. WEB AUDIO API SYNTHESIZER (MUSIC & SFX)
           ========================================== */
        let audioCtx = null;
        let musicActive = false;
        let sfxActive = true;
        let nextNoteTime = 0.0;
        let timerID = null;
        let currentBeat = 0;

        function initAudio() {
            if (!audioCtx) {
                try {
                    audioCtx = new (window.AudioContext || window.webkitAudioContext)();
                } catch(e) {
                    console.log('Web Audio API not supported');
                }
            }
            if (audioCtx && audioCtx.state === 'suspended') {
                audioCtx.resume();
            }
        }

        function toggleMusic() {
            initAudio();
            musicActive = !musicActive;
            const btn = document.getElementById('btnMusicToggle');
            if (musicActive) {
                btn.textContent = '🎵 Música: ON';
                btn.classList.remove('muted');
                startCyberpunkBeat();
            } else {
                btn.textContent = '🎵 Música: OFF';
                btn.classList.add('muted');
                stopCyberpunkBeat();
            }
        }

        function toggleSfx() {
            sfxActive = !sfxActive;
            const btn = document.getElementById('btnSfxToggle');
            if (sfxActive) {
                btn.textContent = '🔊 Efeitos: ON';
                btn.classList.remove('muted');
            } else {
                btn.textContent = '🔊 Efeitos: OFF';
                btn.classList.add('muted');
            }
        }

        function playSfx(type) {
            if (!sfxActive) return;
            initAudio();
            if (!audioCtx) return;

            const now = audioCtx.currentTime;
            const osc = audioCtx.createOscillator();
            const gain = audioCtx.createGain();
            osc.connect(gain);
            gain.connect(audioCtx.destination);

            if (type === 'click') {
                osc.type = 'sine';
                osc.frequency.setValueAtTime(800, now);
                osc.frequency.exponentialRampToValueAtTime(400, now + 0.05);
                gain.gain.setValueAtTime(0.1, now);
                gain.gain.linearRampToValueAtTime(0.01, now + 0.05);
                osc.start(now);
                osc.stop(now + 0.05);
            } else if (type === 'success') {
                osc.type = 'triangle';
                osc.frequency.setValueAtTime(523.25, now); // C5
                osc.frequency.setValueAtTime(659.25, now + 0.1); // E5
                osc.frequency.setValueAtTime(783.99, now + 0.2); // G5
                gain.gain.setValueAtTime(0.15, now);
                gain.gain.linearRampToValueAtTime(0.01, now + 0.4);
                osc.start(now);
                osc.stop(now + 0.4);
            } else if (type === 'error') {
                osc.type = 'sawtooth';
                osc.frequency.setValueAtTime(150, now);
                osc.frequency.setValueAtTime(110, now + 0.15);
                gain.gain.setValueAtTime(0.2, now);
                gain.gain.linearRampToValueAtTime(0.01, now + 0.35);
                osc.start(now);
                osc.stop(now + 0.35);
            }
        }

        // Cyberpunk Hip-Hop 90 BPM Scheduler
        function startCyberpunkBeat() {
            if (!audioCtx) return;
            currentBeat = 0;
            nextNoteTime = audioCtx.currentTime + 0.1;
            timerID = setInterval(scheduler, 50);
        }

        function stopCyberpunkBeat() {
            if (timerID) {
                clearInterval(timerID);
                timerID = null;
            }
        }

        function scheduler() {
            if (!audioCtx || !musicActive) return;
            while (nextNoteTime < audioCtx.currentTime + 0.1) {
                scheduleNote(currentBeat, nextNoteTime);
                nextNoteTime += 60.0 / 90.0 / 4.0; // 16th notes at 90 BPM
                currentBeat = (currentBeat + 1) % 32;
            }
        }

        function scheduleNote(beat, time) {
            // Bass / Kick on 0, 8, 16, 24
            if (beat % 8 === 0) {
                let osc = audioCtx.createOscillator();
                let gain = audioCtx.createGain();
                osc.type = 'sine';
                osc.frequency.setValueAtTime(120, time);
                osc.frequency.exponentialRampToValueAtTime(40, time + 0.15);
                gain.gain.setValueAtTime(0.3, time);
                gain.gain.linearRampToValueAtTime(0.01, time + 0.2);
                osc.connect(gain);
                gain.connect(audioCtx.destination);
                osc.start(time);
                osc.stop(time + 0.2);
            }
            // Hi-hat on every odd 16th
            if (beat % 2 !== 0) {
                let node = audioCtx.createBufferSource();
                let buffer = audioCtx.createBuffer(1, audioCtx.sampleRate * 0.05, audioCtx.sampleRate);
                let data = buffer.getChannelData(0);
                for (let i = 0; i < buffer.length; i++) {
                    data[i] = Math.random() * 2 - 1;
                }
                node.buffer = buffer;
                let filter = audioCtx.createBiquadFilter();
                filter.type = 'highpass';
                filter.frequency.value = 5000;
                let gain = audioCtx.createGain();
                gain.gain.setValueAtTime(0.08, time);
                gain.gain.linearRampToValueAtTime(0.001, time + 0.05);
                node.connect(filter);
                filter.connect(gain);
                gain.connect(audioCtx.destination);
                node.start(time);
            }
        }

        /* ==========================================
           2. GAME DATA: MISSIONS & CLINICAL CASES
           ========================================== */
        const allVaccinesList = [
            "BCG", "Hepatite B", "Pentavalente (DTP/HB/Hib)", "VIP (Poliomielite Inativada)", 
            "VORH (Rotavírus Humano)", "Meningocócica C", "Pneumocócica 10-valente", 
            "Febre Amarela", "Tríplice Viral (Sarampo/Caxumba/Rubéola)", "Tetra Viral", 
            "Hepatite A", "DTP (Tríplice Bacteriana)", "Varicela", "HPV Quadrivalente", 
            "Meningocócica ACWY", "dT (Dupla Adulto)", "dTpa (Gestante / Adulto)", "Influenza (Gripe)", "Covid-19"
        ];

        const allBatchesList = [
            "LOTE-A24X (Val: 2028)", "LOTE-B99K (VENCIDO: 2023)", "LOTE-NX77 (Val: 2029)", 
            "LOTE-V880 (VENCIDO: 2024)", "LOTE-BR01 (Val: 2027)"
        ];

        const missionCases = [
            {
                id: 1,
                title: "Missão 01: Distrito Neon (Recém-Nascido)",
                cpf: "111.444.777-01",
                name: "Bebê Neo Silva",
                dob: "Hoje (0 dias)",
                age: "Recém-Nascido",
                condition: "Alojamento Conjunto Maternidade",
                history: [],
                guide: "<strong>Orientações para RN:</strong> Ao nascer, na maternidade, o recém-nascido deve receber a <strong>Vacina BCG</strong> (via intradérmica, na inserção inferior do músculo <strong>deltoide direito</strong>) para prevenir formas graves de tuberculose, e a <strong>Vacina Hepatite B</strong> (via intramuscular, no músculo <strong>vasto lateral da coxa direita</strong>) nas primeiras 24 horas de vida.",
                correct: {
                    action: "vaccinate",
                    vaccine: "Hepatite B",
                    dose: "D1",
                    batch: "LOTE-A24X (Val: 2028)",
                    route: "Intramuscular",
                    anatomic: "Vasto Lateral Coxa Direita",
                    strategy: "Rotina"
                },
                explanation: "Correto! A Hepatite B ao nascer é administrada por via intramuscular no músculo vasto lateral da coxa direita, prevenindo a transmissão vertical."
            },
            {
                id: 2,
                title: "Missão 02: Setor Dystopia (Bebê 2 Meses)",
                cpf: "222.555.888-02",
                name: "Kira dos Santos",
                dob: "Há 60 dias",
                age: "2 Meses",
                condition: "Rotina UBS",
                history: [
                    { vaccine: "Hepatite B", dose: "D1", date: "Há 60 dias", lote: "LOTE-A24X" },
                    { vaccine: "BCG", dose: "DU", date: "Há 60 dias", lote: "LOTE-NX77" }
                ],
                guide: "<strong>Orientações para 2 Meses:</strong> No esquema básico de 2 meses, administre a <strong>Pentavalente</strong> (DTP/HB/Hib) por via intramuscular no vasto lateral da coxa esquerda, <strong>VIP</strong> (Poliomielite Inativada) via intramuscular na coxa direita, <strong>Rotavírus Humano</strong> via oral (gotas) e <strong>Pneumocócica 10-valente</strong> via intramuscular. Cuidado com o lote vencido!",
                correct: {
                    action: "vaccinate",
                    vaccine: "Pentavalente (DTP/HB/Hib)",
                    dose: "D1",
                    batch: "LOTE-A24X (Val: 2028)",
                    route: "Intramuscular",
                    anatomic: "Vasto Lateral Coxa Esquerda",
                    strategy: "Rotina"
                },
                explanation: "Correto! A Pentavalente 1ª dose é aplicada no vasto lateral da coxa esquerda, respeitando o lote válido."
            },
            {
                id: 3,
                title: "Missão 03: Zona de Quarentena (Bebê 4 Meses)",
                cpf: "333.666.999-03",
                name: "Zack Mendonça",
                dob: "Há 120 dias",
                age: "4 Meses",
                condition: "Retorno de rotina",
                history: [
                    { vaccine: "Pentavalente (DTP/HB/Hib)", dose: "D1", date: "Há 60 dias", lote: "LOTE-A24X" },
                    { vaccine: "VIP (Poliomielite Inativada)", dose: "D1", date: "Há 60 dias", lote: "LOTE-NX77" }
                ],
                guide: "<strong>Orientações para 4 Meses:</strong> Verifique o intervalo de 60 dias da D1. Como o intervalo foi cumprido, aplique a <strong>2ª dose da VIP</strong> (Poliomielite Inativada) por via intramuscular no músculo vasto lateral da coxa esquerda.",
                correct: {
                    action: "vaccinate",
                    vaccine: "VIP (Poliomielite Inativada)",
                    dose: "D2",
                    batch: "LOTE-NX77 (Val: 2029)",
                    route: "Intramuscular",
                    anatomic: "Vasto Lateral Coxa Esquerda",
                    strategy: "Rotina"
                },
                explanation: "Correto! A VIP D2 é administrada por via intramuscular na coxa esquerda, mantendo o intervalo adequado de 60 dias."
            },
            {
                id: 4,
                title: "Missão 04: Sub-Nível Alpha (Criança 1 Ano)",
                cpf: "444.777.000-04",
                name: "Luna Matrix",
                dob: "Há 365 dias",
                age: "12 Meses (1 ano)",
                condition: "Campanha e Rotina",
                history: [
                    { vaccine: "Pentavalente (DTP/HB/Hib)", dose: "D3", date: "Há 6 meses", lote: "LOTE-A24X" },
                    { vaccine: "VIP (Poliomielite Inativada)", dose: "D3", date: "Há 6 meses", lote: "LOTE-NX77" }
                ],
                guide: "<strong>Orientações para 12 Meses:</strong> O calendário indica a <strong>Tríplice Viral (SCR)</strong> (sarampo, caxumba, rubéola) por via subcutânea no deltoide direito, além dos reforços de Pneumocócica e Meningocócica C. Cuidado com lotes vencidos!",
                correct: {
                    action: "vaccinate",
                    vaccine: "Tríplice Viral (Sarampo/Caxumba/Rubéola)",
                    dose: "D1",
                    batch: "LOTE-BR01 (Val: 2027)",
                    route: "Subcutânea",
                    anatomic: "Deltoide Direito",
                    strategy: "Rotina"
                },
                explanation: "Correto! A Tríplice Viral aos 12 meses protege contra sarampo, caxumba e rubéola via subcutânea no deltoide direito."
            },
            {
                id: 5,
                title: "Missão 05: Complexo Industrial (Criança 15 Meses)",
                cpf: "555.888.111-05",
                name: "Neo Jr. Alcantara",
                dob: "Há 450 dias",
                age: "15 Meses",
                condition: "Pequeno atraso vacinal",
                history: [
                    { vaccine: "Tríplice Viral (Sarampo/Caxumba/Rubéola)", dose: "D1", date: "Há 3 meses", lote: "LOTE-BR01" }
                ],
                guide: "<strong>Orientações para 15 Meses:</strong> Aos 15 meses aplica-se o 1º reforço da DTP (Tríplice Bacteriana) por via intramuscular no vasto lateral da coxa direita e a vacina <strong>Tetra Viral</strong> (ou Hepatite A e Varicela).",
                correct: {
                    action: "vaccinate",
                    vaccine: "DTP (Tríplice Bacteriana)",
                    dose: "Reforço",
                    batch: "LOTE-A24X (Val: 2028)",
                    route: "Intramuscular",
                    anatomic: "Vasto Lateral Coxa Direita",
                    strategy: "Rotina"
                },
                explanation: "Correto! O 1º reforço da DTP aos 15 meses é aplicado via intramuscular na coxa direita."
            },
            {
                id: 6,
                title: "Missão 06: Megatorre Central (Adolescente)",
                cpf: "666.999.222-06",
                name: "Aria Synth",
                dob: "14 Anos atrás",
                age: "14 Anos",
                condition: "Campanha Escolar",
                history: [
                    { vaccine: "Tríplice Viral (Sarampo/Caxumba/Rubéola)", dose: "D2", date: "Há 8 anos", lote: "LOTE-BR01" }
                ],
                guide: "<strong>Orientações para Adolescentes:</strong> Na faixa de 9 a 14 anos, recomenda-se a dose única da <strong>HPV Quadrivalente</strong> por via intramuscular no músculo deltoide esquerdo.",
                correct: {
                    action: "vaccinate",
                    vaccine: "HPV Quadrivalente",
                    dose: "Dose Única",
                    batch: "LOTE-NX77 (Val: 2029)",
                    route: "Intramuscular",
                    anatomic: "Deltoide Esquerdo",
                    strategy: "Campanha"
                },
                explanation: "Correto! A HPV Quadrivalente é administrada no deltoide esquerdo para prevenção de verrugas genitais e cânceres associados."
            },
            {
                id: 7,
                title: "Missão 07: Setor Residencial (Gestante 24 semanas)",
                cpf: "777.000.333-07",
                name: "Valéria Vex",
                dob: "28 Anos atrás",
                age: "28 Anos",
                condition: "Gestante (24 semanas)",
                history: [
                    { vaccine: "dT (Dupla Adulto)", dose: "Reforço", date: "Há 4 anos", lote: "LOTE-BR01" }
                ],
                guide: "<strong>Orientações para Gestantes:</strong> A partir da 20ª semana de gestação, a gestante deve receber a <strong>dTpa</strong> (Difteria, Tétano e Coqueluche acelular) por via intramuscular no deltoide direito para proteger o concepto contra coqueluche neonatal.",
                correct: {
                    action: "vaccinate",
                    vaccine: "dTpa (Gestante / Adulto)",
                    dose: "Dose Única",
                    batch: "LOTE-A24X (Val: 2028)",
                    route: "Intramuscular",
                    anatomic: "Deltoide Direito",
                    strategy: "Rotina"
                },
                explanation: "Correto! A dTpa na gestação (a partir da 20ª semana) protege a mãe e transfere anticorpos contra a coqueluche para o recém-nascido via intramuscular no deltoide."
            },
            {
                id: 8,
                title: "Missão 08: Posto Avançado de Fronteira (Viajante)",
                cpf: "888.111.444-08",
                name: "Marcus Nomad",
                dob: "35 Anos atrás",
                age: "35 Anos",
                condition: "Viajante para Área Endêmica",
                history: [
                    { vaccine: "Hepatite B", dose: "D3", date: "Há 5 anos", lote: "LOTE-BR01" }
                ],
                guide: "<strong>Orientações para Viajantes:</strong> Para áreas com recomendação de febre amarela, aplica-se a <strong>Febre Amarela (atenuada)</strong> por via subcutânea no deltoide esquerdo. ATENÇÃO: Verifique se o lote selecionado não está vencido!",
                correct: {
                    action: "vaccinate",
                    vaccine: "Febre Amarela",
                    dose: "Dose Única",
                    batch: "LOTE-NX77 (Val: 2029)",
                    route: "Subcutânea",
                    anatomic: "Deltoide Esquerdo",
                    strategy: "Rotina"
                },
                explanation: "Correto! A vacina de Febre Amarela é administrada por via subcutânea no deltoide esquerdo, com lote válido."
            },
            {
                id: 9,
                title: "Missão 09: Zona Neutra (Idoso 68 anos)",
                cpf: "999.222.555-09",
                name: "Silas Core",
                dob: "68 Anos atrás",
                age: "68 Anos",
                condition: "Campanha Sazonal Idosos",
                history: [
                    { vaccine: "dT (Dupla Adulto)", dose: "Reforço", date: "Há 2 anos", lote: "LOTE-BR01" }
                ],
                guide: "<strong>Orientações para Idosos:</strong> Na campanha anual contra influenza, o idoso recebe 1 dose da <strong>Influenza (Gripe)</strong> por via intramuscular no deltoide direito. Note que o histórico mostra dT recente, então não precisa de dT agora.",
                correct: {
                    action: "vaccinate",
                    vaccine: "Influenza (Gripe)",
                    dose: "Anual",
                    batch: "LOTE-BR01 (Val: 2027)",
                    route: "Intramuscular",
                    anatomic: "Deltoide Direito",
                    strategy: "Campanha"
                },
                explanation: "Correto! A vacina Influenza protege contra cepas sazonais da gripe via intramuscular no deltoide direito."
            },
            {
                id: 10,
                title: "Missão 10: Chefão Final // Cânion Ciber-Médico",
                cpf: "000.333.666-10",
                name: "Dra. Nyx Cipher",
                dob: "40 Anos atrás",
                age: "40 Anos",
                condition: "Profissional de Saúde (Com contraindicação temporária)",
                history: [
                    { vaccine: "Hepatite B", dose: "D3", date: "Há 1 ano", lote: "LOTE-BR01" },
                    { vaccine: "dT (Dupla Adulto)", dose: "Reforço", date: "Há 3 anos", lote: "LOTE-BR01" }
                ],
                guide: "<strong>CASO COMPLEXO (CHEFÃO):</strong> Dra. Nyx comparece à sala de vacina referindo febre alta aguda (38.8°C) e exantema alérgico agudo. Ela precisa de reforço de dT? <strong>NÃO!</strong> Doença aguda febrere grave é contraindicação temporária para vacinação. O correto é registrar a contraindicação clínica.",
                correct: {
                    action: "contraindication",
                    vaccine: "",
                    dose: "",
                    batch: "",
                    route: "",
                    anatomic: "",
                    strategy: "",
                    reason: "Contraindicacao Clinica"
                },
                explanation: "Excelente! Condições agudas febris moderadas a graves constituem adiamento/contraindicação temporária da vacinação até a resolução do quadro clínico."
            }
        ];

        /* ==========================================
           3. GAME STATE CONTROLLER
           ========================================== */
        let currentMissionIndex = 0;
        let playerHp = 100;
        let playerXp = 0;

        function startGame() {
            playSfx('click');
            currentMissionIndex = 0;
            playerHp = 100;
            playerXp = 0;
            updateHud();
            switchScreen('screenMission');
            loadMissionLogin();
        }

        function restartGame() {
            playSfx('click');
            startGame();
        }

        function switchScreen(screenId) {
            document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
            document.getElementById(screenId).classList.add('active');
        }

        function updateHud() {
            document.getElementById('hudMissionNum').textContent = `${currentMissionIndex + 1} / 10`;
            document.getElementById('hpBar').style.width = `${playerHp}%`;
            document.getElementById('hudXp').textContent = `${playerXp} PTS`;
        }

        function updateMascot(text) {
            document.getElementById('mascotText').innerHTML = `<strong>Byte:</strong> ${text}`;
        }

        /* ==========================================
           4. MISSION WORKFLOW & TERMINAL SIMULATION
           ========================================== */
        function loadMissionLogin() {
            const mission = missionCases[currentMissionIndex];
            document.getElementById('subScreenLogin').style.display = 'block';
            document.getElementById('subScreenSearch').style.display = 'none';
            document.getElementById('subScreenWorkspace').style.display = 'none';

            updateMascot(`Missão ${currentMissionIndex + 1} iniciada. Faça o login no terminal SI-PNI para acessar o sistema.`);
        }

        function processLogin() {
            playSfx('click');
            playSfx('success');
            document.getElementById('subScreenLogin').style.display = 'none';
            document.getElementById('subScreenSearch').style.display = 'block';
            updateMascot("Login autenticado com sucesso! Agora digite ou preencha automaticamente o CPF do cidadão.");
        }

        function fillCpf() {
            playSfx('click');
            const mission = missionCases[currentMissionIndex];
            document.getElementById('searchInput').value = mission.cpf;
            updateMascot("CPF preenchido com sucesso. Clique em 'Buscar Paciente' para abrir o prontuário.");
        }

        function executeSearch() {
            playSfx('click');
            const inputVal = document.getElementById('searchInput').value.trim();
            const mission = missionCases[currentMissionIndex];

            if (inputVal !== mission.cpf) {
                playSfx('error');
                updateMascot("⚠️ Cidadão não encontrado ou CPF incorreto! Tente novamente ou clique em 'Preencher CPF'.");
                alert("SI-PNI // ERRO: Cidadão não encontrado no banco de dados municipal!");
                return;
            }

            playSfx('success');
            document.getElementById('subScreenSearch').style.display = 'none';
            document.getElementById('subScreenWorkspace').style.display = 'grid';

            // Populate Patient Sheet
            document.getElementById('ptName').textContent = mission.name;
            document.getElementById('ptDob').textContent = mission.dob;
            document.getElementById('ptAge').textContent = mission.age;
            document.getElementById('ptCondition').textContent = mission.condition;

            const tbody = document.getElementById('ptHistoryBody');
            tbody.innerHTML = '';
            if (mission.history.length === 0) {
                tbody.innerHTML = `<tr><td colspan="4" style="text-align:center; color: var(--text-dim);">Nenhum registro prévio encontrado no histórico.</td></tr>`;
            } else {
                mission.history.forEach(h => {
                    tbody.innerHTML += `<tr><td>${h.vaccine}</td><td>${h.dose}</td><td>${h.date}</td><td>${h.lote}</td></tr>`;
                });
            }

            // Populate Guide Panel
            document.getElementById('guideContentText').innerHTML = mission.guide;

            // Populate Form Dropdowns
            populateDropdowns();

            updateMascot(`Prontuário de ${mission.name} carregado. Leia atentamente o Guia Clínico do Ministério da Saúde acima e preencha todos os campos do formulário com atenção especial ao <strong>lado anatômico</strong>!`);
        }

        function populateDropdowns() {
            // Vaccines
            const vaxSelect = document.getElementById('fieldVaccine');
            vaxSelect.innerHTML = '<option value="">-- Selecione a Vacina --</option>';
            allVaccinesList.forEach(v => {
                vaxSelect.innerHTML += `<option value="${v}">${v}</option>`;
            });

            // Batches (shuffled)
            const batchSelect = document.getElementById('fieldBatch');
            batchSelect.innerHTML = '<option value="">-- Selecione o Lote --</option>';
            const shuffledBatches = [...allBatchesList].sort(() => Math.random() - 0.5);
            shuffledBatches.forEach(b => {
                batchSelect.innerHTML += `<option value="${b}">${b}</option>`;
            });
        }

        function toggleActionFields() {
            const action = document.getElementById('fieldAction').value;
            const isVaccinate = (action === 'vaccinate');

            document.getElementById('groupVaccine').style.display = isVaccinate ? 'flex' : 'none';
            document.getElementById('groupDose').style.display = isVaccinate ? 'flex' : 'none';
            document.getElementById('groupBatch').style.display = isVaccinate ? 'flex' : 'none';
            document.getElementById('groupRoute').style.display = isVaccinate ? 'flex' : 'none';
            document.getElementById('groupAnatomic').style.display = isVaccinate ? 'flex' : 'none';
            document.getElementById('groupStrategy').style.display = isVaccinate ? 'flex' : 'none';
            document.getElementById('groupReason').style.display = isVaccinate ? 'none' : 'flex';
        }

        function triggerHint() {
            playSfx('click');
            const mission = missionCases[currentMissionIndex];
            if (mission.correct.action === 'contraindication') {
                updateMascot("💡 <strong>Dica do Byte:</strong> Este caso apresenta contraindicação clínica! Mude a decisão para 'Registrar Contraindicação'.");
            } else {
                updateMascot(`💡 <strong>Dica do Byte:</strong> A vacina correta é <strong>${mission.correct.vaccine}</strong>, aplicada via <strong>${mission.correct.route}</strong> no local <strong>${mission.correct.anatomic}</strong>.`);
            }
        }

        /* ==========================================
           5. FORM VALIDATION & CLINICAL FEEDBACK
           ========================================== */
        function submitVaccineForm(event) {
            event.preventDefault();
            const mission = missionCases[currentMissionIndex];
            const correct = mission.correct;

            const action = document.getElementById('fieldAction').value;
            const vaccine = document.getElementById('fieldVaccine').value;
            const dose = document.getElementById('fieldDose').value;
            const batch = document.getElementById('fieldBatch').value;
            const route = document.getElementById('fieldRoute').value;
            const anatomic = document.getElementById('fieldAnatomic').value;
            const strategy = document.getElementById('fieldStrategy').value;
            const reason = document.getElementById('fieldReason').value;

            let errors = [];

            if (action !== correct.action) {
                errors.push("Decisão clínica incorreta para este paciente.");
            } else if (action === 'vaccinate') {
                if (vaccine !== correct.vaccine) errors.push(`Imunobiológico incorreto. O correto seria: ${correct.vaccine}`);
                if (dose !== correct.dose) errors.push(`Dose incorreta. A correta seria: ${correct.dose}`);
                if (batch !== correct.batch) errors.push(`Lote incorreto ou vencido selecionado! Verifique a validade.`);
                if (route !== correct.route) errors.push(`Via de administração incorreta. A correta seria: ${correct.route}`);
                if (anatomic !== correct.anatomic) errors.push(`Lado / Local anatômico incorreto. O recomendado pelo Ministério da Saúde é: ${correct.anatomic}`);
                if (strategy !== correct.strategy) errors.push(`Estratégia de vacinação incorreta.`);
            } else {
                if (reason !== correct.reason) errors.push("Motivo de não vacinação incorreto.");
            }

            const modalOverlay = document.getElementById('feedbackModal');
            const modalTitle = document.getElementById('modalTitle');
            const modalBody = document.getElementById('modalBody');

            modalOverlay.style.display = 'flex';

            if (errors.length === 0) {
                playSfx('success');
                playerXp += 150;
                modalTitle.textContent = "✨ MISSÃO CUMPRIDA // REGISTRO ACEITO";
                modalTitle.className = "modal-title";
                modalBody.innerHTML = `
                    <p style="color: var(--success); font-weight: bold; margin-bottom: 10px;">+150 XP Registrados no SI-PNI com Sucesso!</p>
                    <p>${mission.explanation}</p>
                `;
                updateMascot("Excelente trabalho! O registro passou por todas as validações epidemiológicas.");
            } else {
                playSfx('error');
                playerHp = Math.max(0, playerHp - 25);
                modalTitle.textContent = "❌ ACESSO NEGADO // ERRO CLÍNICO";
                modalTitle.className = "modal-title error";
                modalBody.innerHTML = `
                    <p style="color: var(--error); font-weight: bold; margin-bottom: 10px;">Inconsistências encontradas no preenchimento:</p>
                    <ul style="padding-left: 20px; margin-bottom: 10px;">
                        ${errors.map(e => `<li>${e}</li>`).join('')}
                    </ul>
                    <p><strong>Fundamentação Clínica:</strong> ${mission.explanation}</p>
                `;
                updateMascot("Atenção aos detalhes clínicos e anatômicos! Revise as normas do PNI.");
            }

            updateHud();
        }

        function closeModalAndProceed() {
            playSfx('click');
            document.getElementById('feedbackModal').style.display = 'none';

            if (playerHp <= 0) {
                alert("Integridade do sistema esgotada (HP Zero). Vamos reiniciar o treinamento para reforçar os conceitos.");
                startGame();
                return;
            }

            currentMissionIndex++;
            if (currentMissionIndex < missionCases.length) {
                loadMissionLogin();
            } else {
                // Final Summary
                switchScreen('screenSummary');
                document.getElementById('summaryScore').textContent = `${playerXp} PTS`;
                document.getElementById('summaryCompleted').textContent = `10 / 10`;
                updateMascot("Parabéns! Você concluiu todas as missões e se tornou um(a) Especialista em SI-PNI!");
            }
        }
    </script>
</body>
</html>

