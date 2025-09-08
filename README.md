<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Controle de CTPS e Metas</title>
    <!-- Tailwind CSS para estilização rápida e responsiva -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- html2canvas para a funcionalidade de exportar como imagem -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
    <style>
        /* Variáveis de cores para o modo escuro (padrão) */
        :root {
            --bg-primary: #0f172a;
            --bg-secondary: #1e293b;
            --bg-card: rgba(30, 41, 59, 0.7);
            --bg-card-hover: rgba(31, 42, 60, 0.75);
            --bg-table-header: rgba(30, 41, 59, 0.75);
            --bg-input: #334155;
            --bg-modal: rgba(31, 41, 59, 0.85);
            --text-primary: #e2e8f0;
            --text-secondary: #94a3b8;
            --border-primary: #475569;
            --border-secondary: #334155;
            --accent-teal: #2dd4bf;
            --accent-blue: #3b82f6;
            --accent-green: #22c55e;
            --accent-red: #ef4444;
            --scrollbar-thumb: #475569;
            --scrollbar-track: #1e293b;
            --bg-secondary-rgb: 30, 41, 59;
        }

        /* Variáveis de cores para o modo claro */
        body.light-mode {
            --bg-primary: #f1f5f9;
            --bg-secondary: #e2e8f0;
            --bg-card: rgba(248, 250, 252, 0.75);
            --bg-card-hover: rgba(238, 242, 246, 0.8);
            --bg-table-header: rgba(226, 232, 240, 0.7);
            --bg-input: #f8fafc;
            --bg-modal: rgba(248, 250, 252, 0.85);
            --text-primary: #1e293b;
            --text-secondary: #475569;
            --border-primary: #cbd5e1;
            --border-secondary: #e2e8f0;
            --accent-teal: #0f766e;
            --accent-blue: #2563eb;
            --accent-green: #16a34a;
            --accent-red: #dc2626;
            --scrollbar-thumb: #a1a1aa;
            --scrollbar-track: #e4e4e7;
            --bg-secondary-rgb: 226, 232, 240;
        }

        /* Estilos customizados para aprimorar a aparência e a responsividade */
        /* Para navegadores WebKit (Chrome, Safari, etc.) */
        ::-webkit-scrollbar {
            width: 8px;
            height: 8px;
        }
        ::-webkit-scrollbar-track {
            background: var(--scrollbar-track);
        }
        ::-webkit-scrollbar-thumb {
            background-color: var(--scrollbar-thumb);
            border-radius: 10px;
            border: 2px solid var(--scrollbar-track);
        }
        ::-webkit-scrollbar-thumb:hover {
            background-color: #64748b;
        }

        /* Estilos para Firefox */
        html {
            scrollbar-width: thin;
            scrollbar-color: var(--scrollbar-thumb) var(--scrollbar-track);
        }

        body {
            font-family: 'Inter', sans-serif;
            color: var(--text-primary);
            background-image: url('https://sgimage.netmarble.com/images/netmarble/mherosgb/20250718/6ou31752803599025.jpg');
            background-size: cover;
            background-repeat: no-repeat;
            background-attachment: fixed;
            background-position: center center;
            position: relative;
            z-index: 1;
        }

        body::before {
            content: '';
            position: fixed;
            top: 0; left: 0; right: 0; bottom: 0;
            background-color: rgba(15, 23, 42, 0.8);
            z-index: -1;
            transition: background-color 0.3s ease;
        }

        body.light-mode::before {
            background-color: rgba(241, 245, 249, 0.8);
        }

        .color-palette-nav {
            transform: translateX(-100%);
            transition: transform 0.3s ease-in-out;
            z-index: 40;
            background-color: rgba(var(--bg-secondary-rgb), 0.7);
            border-right: 1px solid var(--border-primary);
            backdrop-filter: blur(12px);
        }
        body.light-mode .color-palette-nav {
            background-color: rgba(var(--bg-secondary-rgb), 0.8);
            border-right-color: var(--border-primary);
        }

        .color-palette-nav.open {
            transform: translateX(0);
        }

        @media (min-width: 768px) {
            .color-palette-nav {
                transform: translateX(0);
            }
        }
        
        /* Estilos do Menu de Cores Lateral */
        .ctp-category-image {
            display: block;
            width: 40px;
            height: 40px;
            margin: 1rem auto 0.5rem;
            object-fit: contain;
        }
        .ctp-group-title {
            font-size: 0.625rem; /* 10px - Reduzido para melhor encaixe */
            font-weight: 600;
            color: var(--text-secondary);
            text-transform: uppercase;
            letter-spacing: 0.025em; /* Espaçamento entre letras reduzido */
            padding-left: 0;
            margin-bottom: 0.75rem;
            margin-top: 0.25rem; /* Margem superior reduzida */
            text-align: center;
            white-space: nowrap; /* Impede a quebra de linha do título */
        }
        .ctp-group-title:first-of-type {
            margin-top: 0.25rem;
        }
        .color-group {
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
            gap: 0.5rem;
        }

        .color-box {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 0.25rem;
            padding: 0.25rem;
            border-radius: 0.5rem;
            transition: background-color 0.2s;
            cursor: grab;
            width: 70px;
        }
        .color-box:hover {
            background-color: var(--bg-card-hover);
        }
        .dragging {
            opacity: 0.5;
            transform: scale(1.05);
        }
        .color-box .color-circle {
            width: 2.5rem; /* 40px */
            height: 2.5rem; /* 40px */
            border-radius: 9999px;
            flex-shrink: 0;
            border: 2px solid rgba(255,255,255,0.1);
        }
        .color-box .color-box-text {
            font-size: 0.75rem; /* 12px */
            font-weight: 500;
            color: var(--text-primary);
            text-align: center;
            line-height: 1;
        }

        .glow-on-focus:focus-within {
            box-shadow: 0 0 8px 2px var(--accent-teal);
        }

        /* Estilo para a barra de pesquisa customizada */
        .character-search-input {
            width: 100%;
            padding: 0.5rem;
            background-color: var(--bg-input);
            border: 1px solid var(--border-primary);
            border-radius: 0.375rem;
            color: var(--text-primary);
            outline: none;
            transition: background-color 0.3s ease, border-color 0.3s ease, color 0.3s ease;
        }
        .character-search-input:focus {
            border-color: var(--accent-teal);
            box-shadow: 0 0 0 2px rgba(45, 212, 191, 0.5);
        }
        
        .toggle-btn {
            background-color: var(--bg-card);
            border: 1px solid var(--border-primary);
            color: var(--text-primary);
            transition: background-color 0.3s ease, color 0.3s ease, border-color 0.3s ease;
        }
        
        /* Estilos para a nova paleta de cores mobile */
        .mobile-color-palette {
            position: absolute;
            z-index: 50;
            display: flex;
            flex-direction: column;
            gap: 4px;
            padding: 8px;
            border-radius: 12px;
            box-shadow: 0 4px 20px rgba(0, 0, 0, 0.3);
            background-color: var(--bg-modal);
            border: 1px solid var(--border-primary);
            backdrop-filter: blur(10px);
            max-height: 250px;
            overflow-y: auto;
            width: 200px;
        }

        .palette-item {
            display: flex;
            align-items: center;
            gap: 12px;
            padding: 8px;
            border-radius: 8px;
            cursor: pointer;
            transition: background-color 0.2s;
        }

        .palette-item:hover {
            background-color: var(--bg-card-hover);
        }
        
        .mobile-color-palette .color-box {
            width: 28px;
            height: 28px;
            border-radius: 50%;
            flex-shrink: 0;
            cursor: pointer;
        }

        .color-name {
            font-size: 0.875rem;
            color: var(--text-primary);
            white-space: nowrap;
        }

    </style>
</head>
<body class="bg-slate-900">

    <div class="relative min-h-screen md:flex">
        <!-- Overlay para o menu mobile -->
        <div id="mobile-menu-overlay" class="fixed inset-0 bg-black bg-opacity-50 z-30 hidden md:hidden"></div>

        <!-- Botão do Menu Hamburguer (visível apenas em telas pequenas) -->
        <button id="mobile-menu-button" class="md:hidden fixed top-4 left-4 z-50 p-2 rounded-md bg-slate-800/80 text-teal-300 backdrop-blur-sm border border-slate-700">
            <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16m-7 6h7"></path>
            </svg>
        </button>
        
        <!-- Botões de Ação no Canto Superior Direito -->
        <div class="fixed top-4 right-4 z-50 flex flex-col items-end gap-2">
            <!-- Botão para alternar o tema -->
            <div class="relative group">
                <button id="theme-toggle" class="p-2 rounded-full toggle-btn shadow-lg">
                    <svg id="theme-icon" class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
                        <path id="theme-icon-path" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 3v1m0 16v1m9-9h-1M4 12H3m15.364 6.364l-.707-.707M6.343 6.343l-.707-.707m12.728 0l-.707.707M6.343 17.657l-.707.707M16 12a4 4 0 11-8 0 4 4 0 018 0z"></path>
                    </svg>
                </button>
                <div class="absolute top-1/2 -translate-y-1/2 right-full mr-2 w-max bg-slate-800 text-white text-xs rounded py-1 px-2 opacity-0 group-hover:opacity-100 transition-opacity pointer-events-none whitespace-nowrap">
                    Alternar Tema
                </div>
            </div>

            <!-- Botão para Redefinir Seleções -->
            <div class="relative group">
                <button id="reset-selections-btn" class="p-2 rounded-full toggle-btn shadow-lg text-[var(--text-secondary)] hover:text-[var(--accent-red)] transition-colors">
                    <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                        <path d="M21 12a9 9 0 0 0-9-9 .94 .94 0 0 0-1 .94V4a1 1 0 0 0 1 1h.09A6.47 6.47 0 0 1 12 18Z"/>
                        <path d="M3 12a9 9 0 0 0 9 9 .94 .94 0 0 0 1-.94V20a1 1 0 0 0-1-1h-.09A6.47 6.47 0 0 1 12 6Z"/>
                    </svg>
                </button>
                <div class="absolute top-1/2 -translate-y-1/2 right-full mr-2 w-max bg-slate-800 text-white text-xs rounded py-1 px-2 opacity-0 group-hover:opacity-100 transition-opacity pointer-events-none whitespace-nowrap">
                    Redefinir Seleções
                </div>
            </div>
        </div>


        <!-- Paleta de Cores (Menu Lateral) -->
        <nav id="color-palette" class="color-palette-nav fixed top-0 left-0 h-full w-64 md:w-28 p-4 overflow-y-auto">
            <h2 class="text-xl font-bold text-center text-[var(--accent-teal)] mb-6 tracking-wider">CTPs</h2>
            
            <div>
                <img src="https://thanosvibs.money/static/assets/items/ctp_rage.png" alt="Ícone de Fúria" class="ctp-category-image">
                <h3 class="ctp-group-title">Fúria</h3>
                <div class="color-group">
                    <div class="color-box" draggable="true" data-color="#6600CC" title="Fúria Brilhante">
                        <div class="color-circle" style="background-color: #6600CC;"></div>
                        <span class="color-box-text">Fúria B.</span>
                    </div>
                    <div class="color-box" draggable="true" data-color="#9966FF" title="Fúria Poderoso">
                        <div class="color-circle" style="background-color: #9966FF;"></div>
                        <span class="color-box-text">Fúria P.</span>
                    </div>
                    <div class="color-box" draggable="true" data-color="#CC99FF" title="Fúria Normal">
                        <div class="color-circle" style="background-color: #CC99FF;"></div>
                        <span class="color-box-text">Fúria N.</span>
                    </div>
                </div>

                <img src="https://thanosvibs.money/static/assets/items/ctp_liberation.png" alt="Ícone de Liberação" class="ctp-category-image">
                <h3 class="ctp-group-title">Liberação</h3>
                <div class="color-group">
                    <div class="color-box" draggable="true" data-color="#CC9900" title="Liberação Brilhante">
                        <div class="color-circle" style="background-color: #CC9900;"></div>
                        <span class="color-box-text">Liberação B.</span>
                    </div>
                    <div class="color-box" draggable="true" data-color="#FFFF00" title="Liberação Poderoso">
                        <div class="color-circle" style="background-color: #FFFF00;"></div>
                        <span class="color-box-text">Liberação P.</span>
                    </div>
                    <div class="color-box" draggable="true" data-color="#FFFF99" title="Liberação Normal">
                        <div class="color-circle" style="background-color: #FFFF99;"></div>
                        <span class="color-box-text">Liberação N.</span>
                    </div>
                </div>

                <img src="https://thanosvibs.money/static/assets/items/ctp_insight.png" alt="Ícone de Percepção" class="ctp-category-image">
                <h3 class="ctp-group-title">Percepção</h3>
                <div class="color-group">
                     <div class="color-box" draggable="true" data-color="#CC0099" title="Percepção Brilhante">
                        <div class="color-circle" style="background-color: #CC0099;"></div>
                        <span class="color-box-text">Percepção B.</span>
                    </div>
                    <div class="color-box" draggable="true" data-color="#FF00FF" title="Percepção Poderoso">
                        <div class="color-circle" style="background-color: #FF00FF;"></div>
                        <span class="color-box-text">Percepção P.</span>
                    </div>
                    <div class="color-box" draggable="true" data-color="#FF99FF" title="Percepção Normal">
                        <div class="color-circle" style="background-color: #FF99FF;"></div>
                        <span class="color-box-text">Percepção N.</span>
                    </div>
                </div>

                <img src="https://thanosvibs.money/static/assets/items/ctp_competition.png" alt="Ícone de Competição" class="ctp-category-image">
                <h3 class="ctp-group-title">Competição</h3>
                <div class="color-group">
                    <div class="color-box" draggable="true" data-color="#0000FF" title="Competição Brilhante">
                        <div class="color-circle" style="background-color: #0000FF;"></div>
                        <span class="color-box-text">Competição B.</span>
                    </div>
                    <div class="color-box" draggable="true" data-color="#0066FF" title="Competição Poderoso">
                        <div class="color-circle" style="background-color: #0066FF;"></div>
                        <span class="color-box-text">Competição P.</span>
                    </div>
                    <div class="color-box" draggable="true" data-color="#3399FF" title="Competição Normal">
                        <div class="color-circle" style="background-color: #3399FF;"></div>
                        <span class="color-box-text">Competição N.</span>
                    </div>
                </div>
            </div>
        </nav>

        <!-- Conteúdo Principal -->
        <main class="flex-1 md:ml-28 p-4 sm:p-6 lg:p-8 pt-20 sm:pt-6 lg:pt-8">
            <div class="w-full mx-auto p-4 sm:p-8 rounded-2xl shadow-2xl border transition-colors duration-300 backdrop-blur-lg"
                 style="background-color: var(--bg-card); border-color: var(--border-primary);">
                <header class="text-center mb-8">
                    <h1 class="text-2xl sm:text-3xl lg:text-4xl font-bold text-[var(--accent-teal)] tracking-wide">CONTROLE DE CTP'S E METAS</h1>
                    <p class="mt-2" style="color: var(--text-secondary);">BAX | BAL</p>
                    <p class="mt-4 font-light italic text-sm" style="color: var(--text-secondary);">
                        <span class="hidden sm:inline">Arraste uma cor do menu e solte nas células para pintar. Dê um clique duplo para remover.</span>
                        <span class="sm:hidden">Pressione e segure uma célula para colorir. Dê um toque duplo para remover a cor.</span>
                    </p>
                </header>
                
                <div class="flex flex-col md:flex-row items-center justify-center flex-wrap gap-4 mb-8">
                    <div class="w-full md:w-auto md:max-w-sm relative group">
                        <div class="absolute inset-y-0 left-0 pl-3 flex items-center pointer-events-none">
                            <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-slate-400 group-focus-within:text-[var(--accent-teal)] transition-colors" viewBox="0 0 20 20" fill="currentColor">
                                <path fill-rule="evenodd" d="M10 9a3 3 0 100-6 3 3 0 000 6zm-7 9a7 7 0 1114 0H3z" clip-rule="evenodd" />
                            </svg>
                        </div>
                        <input id="nickname-input" type="text" placeholder="Seu Nick no Jogo" class="w-full pl-10 pr-4 py-3 text-center bg-[var(--bg-input)] border border-[var(--border-secondary)] rounded-lg placeholder-slate-400 focus:outline-none focus:ring-2 focus:ring-[var(--accent-teal)] focus:border-[var(--accent-teal)] transition-all" style="color: var(--text-primary);" readonly>
                    </div>
                
                    <button id="manage-characters-button" class="w-full md:w-auto flex items-center justify-center px-6 py-3 border-2 border-[var(--accent-blue)] text-[var(--accent-blue)] font-bold rounded-lg shadow-md hover:bg-[var(--accent-blue)] hover:text-white focus:outline-none focus:ring-4 focus:ring-blue-500/50 transition-all duration-300 transform hover:scale-105">
                        <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 mr-2" viewBox="0 0 20 20" fill="currentColor"><path d="M9 6a3 3 0 11-6 0 3 3 0 016 0zM17 6a3 3 0 11-6 0 3 3 0 016 0zM12.93 17c.046-.327.07-.66.07-1a6.97 6.97 0 00-1.5-4.33A5 5 0 0110 14.25a5 5 0 01-3.43-1.58 6.97 6.97 0 00-1.5 4.33c0 .34.024.673.07 1h9.86zM12 14a5 5 0 01-10 0v-1a5 5 0 0110 0v1z"/></svg>
                        <span>Gerenciar Personagens</span>
                    </button>
                    
                    <button id="manual-save-button" class="w-full md:w-auto flex items-center justify-center px-6 py-3 border-2 border-[var(--accent-green)] text-[var(--accent-green)] font-bold rounded-lg shadow-md hover:bg-[var(--accent-green)] hover:text-white focus:outline-none focus:ring-4 focus:ring-green-500/50 transition-all duration-300 transform hover:scale-105">
                        <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 mr-2" viewBox="0 0 20 20" fill="currentColor"><path fill-rule="evenodd" d="M10 18a8 8 0 100-16 8 8 0 000 16zm3.707-9.293a1 1 0 00-1.414-1.414L9 10.586 7.707 9.293a1 1 0 00-1.414 1.414l2 2a1 1 0 001.414 0l4-4z" clip-rule="evenodd" /></svg>
                        <span>Salvar Progresso</span>
                    </button>
                
                    <button id="export-combined-image-button" class="w-full md:w-auto flex items-center justify-center px-6 py-3 border-2 border-[var(--accent-teal)] text-[var(--accent-teal)] font-bold rounded-lg shadow-md hover:bg-[var(--accent-teal)] hover:text-white focus:outline-none focus:ring-4 focus:ring-teal-500/50 transition-all duration-300 transform hover:scale-105">
                        <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 mr-2" viewBox="0 0 20 20" fill="currentColor"><path fill-rule="evenodd" d="M3 17a1 1 0 011-1h12a1 1 0 110 2H4a1 1 0 01-1-1zm3.293-7.707a1 1 0 011.414 0L9 10.586V3a1 1 0 112 0v7.586l1.293-1.293a1 1 0 111.414 1.414l-3 3a1 1 0 01-1.414 0l-3-3a1 1 0 010-1.414z" clip-rule="evenodd" /></svg>
                        <span>EXPORTAR IMAGEM</span>
                    </button>
                </div>

                <div id="table-container" class="space-y-10">
                    <!-- As tabelas BAX e BAL serão renderizadas aqui pelo JavaScript -->
                </div>

                <!-- Contêiner para as novas tabelas de ranking -->
                <div id="ranking-tables-container" class="mt-10 flex flex-col md:flex-row gap-8">
                    <!-- O conteúdo será gerado dinamicamente pelo JavaScript -->
                </div>
            </div>
        </main>
    </div>

    <!-- Caixa de Mensagem (Toast) -->
    <div id="message-box" class="fixed bottom-4 right-4 p-4 rounded-lg shadow-lg text-white opacity-0 hidden transition-all duration-300 transform translate-y-4">
        <span id="message-text"></span>
    </div>

    <!-- Modal de Confirmação -->
    <div id="confirm-modal" class="fixed inset-0 bg-slate-900/80 flex items-center justify-center hidden z-50 p-4">
        <div class="p-6 rounded-lg shadow-xl border w-full max-w-sm text-center"
             style="background-color: var(--bg-modal); border-color: var(--border-primary); backdrop-filter: blur(12px);">
            <p id="modal-message" class="text-lg font-semibold mb-6" style="color: var(--text-primary);"></p>
            <div class="flex justify-center space-x-4">
                <button id="confirm-btn" class="px-6 py-2 bg-[var(--accent-red)] text-white rounded-md shadow-md hover:bg-red-700 transition-colors duration-200 font-semibold">Confirmar</button>
                <button id="cancel-btn" class="px-6 py-2 bg-[var(--border-primary)] text-white rounded-md shadow-md hover:bg-slate-700 transition-colors duration-200 font-semibold">Cancelar</button>
            </div>
        </div>
    </div>

    <!-- Modal de Gerenciamento de Personagens -->
    <div id="manage-characters-modal" class="fixed inset-0 bg-slate-900/80 flex items-center justify-center hidden z-50 p-4">
        <div class="p-6 rounded-lg shadow-xl border w-full max-w-lg"
             style="background-color: var(--bg-modal); border-color: var(--border-primary); backdrop-filter: blur(12px);">
            <h2 class="text-2xl font-bold text-center text-[var(--accent-teal)] mb-6">Gerenciar Personagens</h2>
            <div class="mb-4">
                <label for="day-select" class="block text-sm font-medium mb-2" style="color: var(--text-secondary);">Selecione o Dia:</label>
                <select id="day-select" class="w-full p-2 rounded-md border focus:outline-none focus:ring focus:border-blue-500"
                        style="background-color: var(--bg-input); color: var(--text-primary); border-color: var(--border-primary);">
                </select>
            </div>
            
            <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                <!-- Seção de Soladores -->
                <div>
                    <h3 class="text-lg font-semibold mb-2" style="color: var(--text-primary);">Soladores</h3>
                    <div class="flex gap-2 mb-2">
                        <input type="text" id="add-solador-input" placeholder="Novo Solador" class="character-search-input flex-1">
                        <button id="add-solador-btn" class="px-4 py-2 bg-[var(--accent-green)] text-white rounded-md hover:bg-green-700 transition-colors">+</button>
                    </div>
                    <ul id="solador-list" class="space-y-2 max-h-48 overflow-y-auto pr-2"></ul>
                </div>

                <!-- Seção de Suportes -->
                <div>
                    <h3 class="text-lg font-semibold mb-2" style="color: var(--text-primary);">Suportes</h3>
                    <div class="flex gap-2 mb-2">
                        <input type="text" id="add-suporte-input" placeholder="Novo Suporte" class="character-search-input flex-1">
                        <button id="add-suporte-btn" class="px-4 py-2 bg-[var(--accent-green)] text-white rounded-md hover:bg-green-700 transition-colors">+</button>
                    </div>
                    <ul id="suporte-list" class="space-y-2 max-h-48 overflow-y-auto pr-2"></ul>
                </div>
            </div>

            <div class="flex justify-end space-x-4 mt-6">
                <button id="save-characters-btn" class="px-6 py-2 bg-[var(--accent-teal)] text-white rounded-md hover:bg-teal-700 transition-colors font-semibold">Salvar e Fechar</button>
                <button id="cancel-manage-btn" class="px-6 py-2 bg-[var(--border-primary)] text-white rounded-md hover:bg-slate-700 transition-colors font-semibold">Cancelar</button>
            </div>
        </div>
    </div>


    <!-- O SCRIPT FOI MOVIDO PARA O FINAL DO BODY PARA MELHOR PERFORMANCE -->
    <script>
        // Dados das tabelas e cores aplicadas
        let tableData = {
            bax: { data: [], colors: {} },
            bal: { data: [], colors: {} }
        };
        let draggingColor = null;

        // --- NOVO: salvar e carregar progresso ---
        const saveProgress = () => {
            try {
                const saveData = {
                    tableData,
                    nickname: document.getElementById('nickname-input').value
                };
                localStorage.setItem('progressData', JSON.stringify(saveData));
            } catch (e) {
                console.error("Erro ao salvar progresso:", e);
            }
        };

        const loadProgress = () => {
            try {
                const stored = localStorage.getItem('progressData');
                if (stored) {
                    const parsed = JSON.parse(stored);
                    if (parsed.tableData) {
                        tableData = parsed.tableData;
                    }
                    if (parsed.nickname) {
                        document.getElementById('nickname-input').value = parsed.nickname;
                    }
                }
            } catch (e) {
                console.error("Erro ao carregar progresso:", e);
            }
        };


        // Dados de entrada em formato CSV para BAX e BAL
        const baxCsv = `Calendário BAX,,,,
Dia,Restrição,Solador,Suporte 1,Suporte 2
1,Vilão | Energia,Doutor Estranho,Mysterio,Encantor
2,Sem Restrição,Storm,Ciclope,Crystal
3,Universal | Herói,Wanda,Satana,Ronan
4,Combate | Herói,Gladiador,Mbaku,Valkyria
5,Energia | Masculino,Doutor Estranho,Mysterio,Coulson
6,Universal | Vilão,Mephisto,Proxima,Morgana
7,Velocidade | Herói,Luna Snow,White Fox,Nick Fury
8,Combate | Vilão,Hulk,Abominável,Red Hulk
9,Sem Restrição,Storm,Ciclope,Crystal
10,Universal | Herói | Feminino,Wanda,Satana,Angela
11,Vilão | Feminino,Gwenpool,Pecado,Gata Negra
12,Velocidade | Herói | Feminino,Luna Snow,White Fox,Misty Knight
13,Combate | Feminino,Crescent,Sif,Valkyria
14,Velocidade | Vilão,Mercenário,Pecado,Gata Negra
`;
        const balCsv = `Calendário BAL,,,,
Dia,Restrição,Solador,Suporte 1,Suporte 2
1,Velocidade | Humano | Feminino,Luna Snow,White Fox,Misty Knight
2,Energia | Vilão | Masculino,Doutor Estranho,Mysterio,Líder
3,Sem Restrições,Doutor Estranho,Pecado,Gata Negra
4,Universal | Feminino,Wanda,Satana,Morgana
5,Humano | Feminino,Luna Snow,White Fox,Misty Knight
6,Combate | Vilão | Masculino,Hulk,Abominável,Red Hulk
7,Energia | Mutante | Herói,Storm,Ciclope,Crystal
8,Velocidade | Feminino,Luna Snow,White Fox,Misty Knight
9,Herói | Alien | Masculino,Gladiador,Groot,Yondu
10,Sem Restrições,Doutor Estranho,Pecado,Gata Negra
11,Herói | Mutante | Feminino,Psylocke,Polaris,Crystal
12,Supervilão | Alien,Loki,Proxima,Encantor
13,Universal | Masculino,Mephisto,Ghost Panther,Ronan
14,Combate | Humano | Masculino,Hulk,Abominável,Red Hulk
`;
        
        // Listas customizadas de personagens
        const defaultCustomLists = {
            'bax-1': { solador: ['Doutor Estranho', 'Magneto'], suporte: ['Líder', 'Encantor', 'Mysterio'] },
            'bax-2': { solador: ['Storm', 'Doutor Estranho', 'Luna Snow', 'Gladiador', 'Wanda', 'Jean Grey', 'Magneto', 'Psylocke', 'Hulk', 'Mercenário', 'Gwenpool', 'Ghost Rider', 'Mephisto'], suporte: ['Ciclope', 'Mistica', 'Nick Fury', 'Treinadora', 'Polaris', 'Chama Solar', 'Abominável', 'Líder', 'Pecado', 'Green Goblin', 'White Fox', 'Misty Knight', 'Yondu', 'Beta Ray Bill', 'Crystal', 'Encantor', 'Red Hulk', 'Sif', 'Angela', 'Ghost Panther', 'Robbie Reyes', 'NOVA (Richard)', 'Phylla-Vel', 'Gata Negra', 'Satana', 'Doutor Voodoo', 'Groot', 'Wong', 'Ancião', 'Proxima', 'Coulson', 'Shuri', 'Morgana', 'Ronan', 'Mbaku', 'Valkyria', 'Mysterio'] },
            'bax-3': { solador: ['Wanda', 'Ghost Rider', 'Capitã Marvel', 'Thor'], suporte: ['Beta Ray Bill', 'Angela', 'Ghost Panther', 'NOVA (Richard)', 'Phylla-Vel', 'Satana', 'Doutor Voodoo', 'Ronan'] },
            'bax-4': { solador: ['Gladiador', 'Venom', 'Crescent', 'Nebula', 'Hope'], suporte: ['Treinadora', 'Sif', 'Agente Venom', 'Groot', 'She Hulk', 'Mbaku', 'Valkyria'] },
            'bax-5': { solador: ['Cable', 'Ciclope', 'Gambit', 'Magneto', 'Starlord', 'Doutor Estranho'], suporte: ['Ciclope', 'Líder', 'Ancião', 'Coulson', 'Mysterio'] },
            'bax-6': { solador: ['Jean Grey', 'Loki', 'Thanos', 'Mephisto'], suporte: ['Hela', 'Proxima', 'Morgana', 'Antiman'] },
            'bax-7': { solador: ['Luna Snow', 'Gamora', 'Vampira', 'Miles Morales', 'Winter Soldier'], suporte: ['Nick Fury', 'White Fox', 'Yondu', 'Shuri', 'Misty Knight', 'Wave'] },
            'bax-8': { solador: ['Hulk', 'Apocalypse', 'Doc Oc'], suporte: ['Abominável', 'Red Hulk'] },
            'bax-9': { solador: ['Storm', 'Doutor Estranho', 'Luna Snow', 'Gladiador', 'Wanda', 'Jean Grey', 'Magneto', 'Psylocke', 'Hulk', 'Mercenário', 'Gwenpool', 'Ghost Rider', 'Mephisto'], suporte: ['Ciclope', 'Mistica', 'Nick Fury', 'Treinadora', 'Polaris', 'Chama Solar', 'Abominável', 'Líder', 'Pecado', 'Green Goblin', 'White Fox', 'Misty Knight', 'Yondu', 'Beta Ray Bill', 'Crystal', 'Encantor', 'Red Hulk', 'Sif', 'Angela', 'Ghost Panther', 'Robbie Reyes', 'NOVA (Richard)', 'Phylla-Vel', 'Gata Negra', 'Satana', 'Doutor Voodoo', 'Groot', 'Wong', 'Wong', 'Ancião', 'Proxima', 'Coulson', 'Shuri', 'Morgana', 'Ronan', 'Mbaku', 'Valkyria', 'Mysterio'] },
            'bax-10': { solador: ['Wanda', 'Capitã Marvel'], suporte: ['Angela', 'Phylla-Vel', 'Satana'] },
            'bax-11': { solador: ['Jean Grey', 'Gwenpool', 'Mistica'], suporte: ['Mistica', 'Pecado', 'Gata Negra', 'Encantor', 'Proxima', 'Morgana'] },
            'bax-12': { solador: ['Gamora', 'Luna Snow', 'Vampira'], suporte: ['White Fox', 'Shuri', 'Misty Knight', 'Wave'] },
            'bax-13': { solador: ['Crescent', 'Hope', 'Nebula', 'Treinadora'], suporte: ['Treinadora', 'Sif', 'She Hulk', 'Valkyria'] },
            'bax-14': { solador: ['Mercenário', 'Mistíca', 'Gwenpool', 'Green Goblin'], suporte: ['Mistica', 'Green Goblin', 'Pecado', 'Zemo', 'Gata Negra'] },
            'bal-1': { solador: ['Luna Snow', 'Gwenpool'], suporte: ['Shuri', 'Pecado', 'White Fox', 'Misty Knight', 'Wave', 'Gata Negra'] },
            'bal-2': { solador: ['Doutor Estranho', 'Magneto'], suporte: ['Líder', 'Mysterio'] },
            'bal-3': { solador: ['Storm', 'Doutor Estranho', 'Luna Snow', 'Gladiador', 'Wanda', 'Jean Grey', 'Magneto', 'Psylocke', 'Hulk', 'Mercenário', 'Gwenpool', 'Ghost Rider', 'Mephisto'], suporte: ['Ciclope', 'Mistica', 'Nick Fury', 'Treinadora', 'Polaris', 'Chama Solar', 'Abominável', 'Líder', 'Pecado', 'Green Goblin', 'White Fox', 'Misty Knight', 'Yondu', 'Beta Ray Bill', 'Crystal', 'Encantor', 'Red Hulk', 'Sif', 'Angela', 'Ghost Panther', 'Robbie Reyes', 'NOVA (Richard)', 'Phylla-Vel', 'Gata Negra', 'Satana', 'Doutor Voodoo', 'Groot', 'Wong', 'Wong', 'Ancião', 'Proxima', 'Coulson', 'Shuri', 'Morgana', 'Ronan', 'Mbaku', 'Valkyria', 'Mysterio'] },
            'bal-4': { solador: ['Wanda', 'Capitã Marvel', 'Jean Grey'], suporte: ['Hela', 'Angela', 'Proxima', 'Morgana', 'Phylla-Vel', 'Satana'] },
            'bal-5': { solador: ['Crescent', 'Luna Snow', 'Capitã Marvel', 'Gwenpool', 'Treinadora'], suporte: ['Shuri', 'Treinadora', 'She Hulk', 'Pecado', 'White Fox', 'Morgana', 'Riri Williams', 'Misty Knight', 'Wave', 'Gata Negra'] },
            'bal-6': { solador: ['Hulk', 'Doc Oc', 'Apocalypse'], suporte: ['Abominável', 'Red Hulk'] },
            'bal-7': { solador: ['Cable', 'Ciclope', 'Gambit', 'Psylocke', 'Storm'], suporte: ['Ciclope', 'Polaris', 'Crystal'] },
            'bal-8': { solador: ['Luna Snow', 'Vampira', 'Gamora', 'Mistica', 'Gwenpool'], suporte: ['Shuri', 'Misty Knight', 'Mistica', 'Pecado', 'White Fox', 'Gata Negra', 'Wave'] },
            'bal-9': { solador: ['Gladiador', 'Starlord', 'Beta Ray Bill', 'Thor'], suporte: ['Starlord', 'Ronan', 'Beta Ray Bill', 'Groot', 'Yondu', 'Rocket'] },
            'bal-10': { solador: ['Storm', 'Doutor Estranho', 'Luna Snow', 'Gladiador', 'Wanda', 'Jean Grey', 'Magneto', 'Psylocke', 'Hulk', 'Mercenário', 'Gwenpool', 'Ghost Rider', 'Mephisto'], suporte: ['Ciclope', 'Mistica', 'Nick Fury', 'Treinadora', 'Polaris', 'Chama Solar', 'Abominável', 'Líder', 'Pecado', 'Green Goblin', 'White Fox', 'Misty Knight', 'Yondu', 'Beta Ray Bill', 'Crystal', 'Encantor', 'Red Hulk', 'Sif', 'Angela', 'Ghost Panther', 'Robbie Reyes', 'NOVA (Richard)', 'Phylla-Vel', 'Gata Negra', 'Satana', 'Doutor Voodoo', 'Groot', 'Wong', 'Wong', 'Ancião', 'Proxima', 'Coulson', 'Shuri', 'Morgana', 'Ronan', 'Mbaku', 'Valkyria', 'Mysterio'] },
            'bal-11': { solador: ['Psylocke', 'Storm', 'Vampira'], suporte: ['Polaris', 'Kitty Pride', 'Crystal'] },
            'bal-12': { solador: ['Loki', 'Knull', 'Thanos'], suporte: ['Hela', 'Encantor', 'Proxima'] },
            'bal-13': { solador: ['Loki', 'Mephisto', 'Thanos', 'Thor', 'Ghost Rider'], suporte: ['Ghost Panther', 'Ronan', 'Beta Ray Bill', 'Doutor Voodoo', 'Ghost Panther', 'Robbie Reyes', 'NOVA (Richard)'] },
            'bal-14': { solador: ['Doc Oc', 'Hulk', 'Venom'], suporte: ['Abominável', 'Mbaku', 'Agente Venom', 'Red Hulk'] }
        };
        
        // Listas de cores para o ranking
        const colorNames = {
            '#6600CC': 'Fúria Brilhante',
            '#9966FF': 'Fúria Poderoso',
            '#CC99FF': 'Fúria Normal',
            '#CC9900': 'Liberação Brilhante',
            '#FFFF00': 'Liberação Poderoso',
            '#FFFF99': 'Liberação Normal',
            '#CC0099': 'Percepção Brilhante',
            '#FF00FF': 'Percepção Poderoso',
            '#FF99FF': 'Percepção Normal',
            '#0000FF': 'Competição Brilhante',
            '#0066FF': 'Competição Poderoso',
            '#3399FF': 'Competição Normal',
        };

        // Carrega as listas de personagens do localStorage ou usa o padrão
        let customLists = {};
        const loadCustomLists = () => {
            try {
                const storedLists = localStorage.getItem('customLists');
                if (storedLists) {
                    customLists = JSON.parse(storedLists);
                } else {
                    customLists = JSON.parse(JSON.stringify(defaultCustomLists)); // Deep copy
                    saveCustomLists();
                }
            } catch (e) {
                console.error('Erro ao carregar do localStorage:', e);
                customLists = JSON.parse(JSON.stringify(defaultCustomLists)); // Deep copy on error
            }
        };

        const saveCustomLists = () => {
            try {
                localStorage.setItem('customLists', JSON.stringify(customLists));
            } catch (e) {
                console.error('Erro ao salvar no localStorage:', e);
            }
        };

        const daysOfWeek = ['QUI', 'SEX', 'SÁB', 'DOM', 'SEG', 'TER', 'QUA'];
        
        const isColorLight = (hex) => {
            if (!hex || typeof hex !== 'string' || hex.length < 4) return false;
            let r, g, b;
            if (hex.length === 4) {
                r = parseInt(hex[1] + hex[1], 16);
                g = parseInt(hex[2] + hex[2], 16);
                b = parseInt(hex[3] + hex[3], 16);
            } else {
                r = parseInt(hex.substring(1, 3), 16);
                g = parseInt(hex.substring(3, 5), 16);
                b = parseInt(hex.substring(5, 7), 16);
            }
            const luminance = (0.299 * r + 0.587 * g + 0.114 * b) / 255;
            return luminance > 0.5;
        };

        const parseCsv = (csvString) => {
            const lines = csvString.trim().split('\n');
            const headerLine = lines[1];
            if (!headerLine) return { headers: [], data: [] };
            const headers = headerLine.split(',').map(h => h.trim()).filter(h => h);
            const dataLines = lines.slice(2);
            const data = dataLines.map(line => {
                const values = line.split(',').map(v => v.trim());
                let obj = {};
                headers.forEach((header, index) => obj[header] = values[index] || '');
                return obj;
            });
            return { headers, data };
        };

        const getCharactersByRole = (baxData, balData) => {
            const soladorCharacters = new Set();
            const suporteCharacters = new Set();
            
            const extractCharacters = (data) => {
                data.forEach(row => {
                    if (row['Solador']) soladorCharacters.add(row['Solador']);
                    if (row['Suporte 1']) suporteCharacters.add(row['Suporte 1']);
                    if (row['Suporte 2']) suporteCharacters.add(row['Suporte 2']);
                });
            };

            extractCharacters(baxData);
            extractCharacters(balData);
            
            return {
                soladors: Array.from(soladorCharacters).sort(),
                suportes: Array.from(suporteCharacters).sort()
            };
        };

        const showMessage = (message, type) => {
            const messageBox = document.getElementById('message-box');
            const messageText = document.getElementById('message-text');
            messageText.textContent = message;
            
            const typeClasses = {
                success: 'bg-[var(--accent-green)]',
                error: 'bg-[var(--accent-red)]',
                info: 'bg-[var(--accent-blue)]'
            };
            
            messageBox.className = `fixed bottom-4 right-4 p-4 rounded-lg shadow-lg text-white transition-all duration-300 transform ${typeClasses[type] || 'bg-gray-600'}`;
            messageBox.classList.remove('opacity-0', 'hidden', 'translate-y-4');
            messageBox.classList.add('opacity-100', 'translate-y-0');

            setTimeout(() => {
                messageBox.classList.remove('opacity-100', 'translate-y-0');
                messageBox.classList.add('opacity-0', 'translate-y-4');
                setTimeout(() => messageBox.classList.add('hidden'), 300);
            }, 3000);
        };
        
        const showConfirmModal = (message, onConfirm) => {
            const modal = document.getElementById('confirm-modal');
            const modalMessage = document.getElementById('modal-message');
            const confirmBtn = document.getElementById('confirm-btn');
            const cancelBtn = document.getElementById('cancel-btn');

            modalMessage.textContent = message;
            modal.classList.remove('hidden');

            const handleConfirm = () => {
                onConfirm();
                modal.classList.add('hidden');
                confirmBtn.removeEventListener('click', handleConfirm);
                cancelBtn.removeEventListener('click', handleCancel);
            };

            const handleCancel = () => {
                modal.classList.add('hidden');
                confirmBtn.removeEventListener('click', handleConfirm);
                cancelBtn.removeEventListener('click', handleCancel);
            };

            confirmBtn.addEventListener('click', handleConfirm);
            cancelBtn.addEventListener('click', handleCancel);
        };

        const createSearchableDropdown = (optionsList, currentValue, onSelectCallback, cell) => {
            const container = document.createElement('div');
            container.className = 'relative inline-block w-full';
            
            const input = document.createElement('input');
            input.className = 'w-full bg-transparent outline-none focus:outline-none cursor-pointer';
            input.readOnly = true;
            input.value = currentValue;
            
            const cellColor = tableData[cell.dataset.tableType].colors[`${cell.dataset.rowIndex}-${cell.dataset.colIndex}-${cell.dataset.tableType}`];
            if (cellColor) {
                input.style.color = isColorLight(cellColor) ? '#0f172a' : '#f8fafc';
            } else {
                input.style.color = getComputedStyle(document.body).getPropertyValue('--text-primary');
            }
            container.appendChild(input);

            const dropdown = document.createElement('div');
            dropdown.className = 'absolute z-50 mt-1 rounded-md shadow-lg max-h-48 overflow-y-auto hidden';
            dropdown.style.backgroundColor = 'var(--bg-modal)';
            dropdown.style.borderColor = 'var(--accent-teal)';
            dropdown.style.color = 'var(--text-primary)';
            
            const searchInput = document.createElement('input');
            searchInput.type = 'text';
            searchInput.placeholder = 'Pesquisar...';
            searchInput.className = 'w-full p-2 sticky top-0 border-b outline-none character-search-input';
            searchInput.style.borderColor = 'var(--accent-teal)';
            searchInput.style.backgroundColor = 'var(--bg-secondary)';
            dropdown.appendChild(searchInput);

            const optionsListDiv = document.createElement('div');
            optionsListDiv.className = 'p-1';
            dropdown.appendChild(optionsListDiv);

            const renderOptions = (filter = '') => {
                optionsListDiv.innerHTML = '';
                const filteredOptions = optionsList.filter(char => char.toLowerCase().includes(filter.toLowerCase()));
                filteredOptions.forEach(character => {
                    const optionDiv = document.createElement('div');
                    optionDiv.className = 'p-2 hover:bg-[var(--accent-teal)] hover:text-[var(--bg-primary)] cursor-pointer rounded-md transition-colors duration-200';
                    optionDiv.textContent = character;
                    optionDiv.addEventListener('click', (e) => {
                        e.stopPropagation();
                        onSelectCallback(character);
                        input.value = character;
                        if (document.body.contains(dropdown)) {
                            document.body.removeChild(dropdown);
                        }
                    });
                    optionsListDiv.appendChild(optionDiv);
                });
            };

            input.addEventListener('click', (e) => {
                e.stopPropagation();
                
                // Fecha qualquer paleta de cores ou dropdown abertos
                document.querySelector('.mobile-color-palette')?.remove();
                document.querySelector('.active-dropdown')?.remove();

                const rect = input.getBoundingClientRect();
                dropdown.style.top = `${rect.bottom + window.scrollY}px`;
                dropdown.style.left = `${rect.left + window.scrollX}px`;
                dropdown.style.width = `${rect.width}px`;
                dropdown.classList.remove('hidden');
                dropdown.classList.add('active-dropdown');
                document.body.appendChild(dropdown);
                searchInput.focus({ preventScroll: true });
                renderOptions();
            });

            searchInput.addEventListener('input', (e) => renderOptions(e.target.value));

            document.addEventListener('click', (e) => {
                if (!dropdown.contains(e.target) && !container.contains(e.target) && document.body.contains(dropdown)) {
                     document.body.removeChild(dropdown);
                }
            });
            
            return container;
        };

        const rgbToHex = (rgb) => {
            if (!rgb) return null;
            const result = /rgb\((\d+),\s*(\d+),\s*(\d+)\)/.exec(rgb);
            return result ? "#" + 
                (1 << 24 | parseInt(result[1]) << 16 | parseInt(result[2]) << 8 | parseInt(result[3])).toString(16).slice(1).toUpperCase() : rgb;
        };
        
        const createTableElement = (data, colors, tableType, charactersByRole, forExport = false) => {
            if (!data || data.length === 0) {
                return '<p class="text-center" style="color: var(--text-secondary);">Nenhum dado para exibir.</p>';
            }

            const titleContent = `
                <div class="flex items-center gap-3">
                    ${tableType.toLowerCase() === 'bax' ? '<img src="https://thanosvibs.money/static/random/frostbeast_silence.png" alt="Frost Beast" class="h-8 w-8 rounded-full object-cover border-2 border-slate-500">' : ''}
                    ${tableType.toLowerCase() === 'bal' ? '<img src="https://thanosvibs.money/static/random/surtur_snare.png" alt="Surtur" class="h-8 w-8 rounded-full object-cover border-2 border-slate-500">' : ''}
                    <h2 class="text-xl font-bold text-[var(--accent-teal)]">${tableType.toUpperCase()}</h2>
                </div>`;

            let tableHtml = `<div class="rounded-lg shadow-md border"
                                 style="background-color: transparent; border-color: var(--border-primary);">
                                <div class="flex flex-col sm:flex-row justify-between items-center p-4 border-b"
                                     style="border-color: var(--border-primary);">
                                    ${titleContent}
                                    ${!forExport ? `<button id="clear-${tableType}-button" class="px-3 py-1 bg-[var(--accent-red)] text-white text-sm font-semibold rounded-md shadow-md hover:bg-red-700 transition-colors duration-200 focus:outline-none focus:ring-2 focus:ring-red-500">Limpar Cores</button>` : ''}
                                </div>
                                <div class="overflow-x-auto">
                                    <table class="w-full text-sm text-left" style="color: var(--text-primary);">
                                        <thead class="text-xs uppercase" style="color: var(--accent-teal); background-color: var(--bg-table-header);">
                                            <tr>
                                                <th scope="col" class="px-3 py-3">Dia</th>
                                                <th scope="col" class="px-3 py-3">Restrição</th>
                                                <th scope="col" class="px-3 py-3">Solador</th>
                                                <th scope="col" class="px-3 py-3">Suporte 1</th>
                                                <th scope="col" class="px-3 py-3">Suporte 2</th>
                                            </tr>
                                        </thead>
                                        <tbody>`;
            
            data.forEach((rowData, rowIndex) => {
                const dayOfWeek = daysOfWeek[rowIndex % 7];
                tableHtml += `<tr class="border-b hover:bg-[var(--bg-card-hover)]" style="border-color: var(--border-secondary);">
                                <td class="px-3 py-3 font-medium whitespace-nowrap">${rowData['Dia']} - ${dayOfWeek}</td>
                                <td class="px-3 py-3">${rowData['Restrição']}</td>`;
                
                ['Solador', 'Suporte 1', 'Suporte 2'].forEach((header, colIndexOffset) => {
                    const colIndex = colIndexOffset + 2;
                    const cellKey = `${rowIndex}-${colIndex}-${tableType}`;
                    const cellColor = colors[cellKey] || '';
                    const cellTextColor = isColorLight(cellColor) ? '#0f172a' : '#f8fafc';

                    if (forExport) {
                        tableHtml += `<td class="px-3 py-3" style="background-color: ${cellColor}; color: ${cellTextColor};">${rowData[header]}</td>`;
                    } else {
                        tableHtml += `<td class="px-3 py-3" data-row-index="${rowIndex}" data-col-index="${colIndex}" data-table-type="${tableType}" id="${cellKey}" style="background-color: ${cellColor};">
                                        <div id="dropdown-container-${cellKey}"></div>
                                     </td>`;
                    }
                });

                tableHtml += `</tr>`;
            });

            tableHtml += `</tbody></table></div></div>`;
            return tableHtml;
        };

        const renderTables = () => {
            const tableContainer = document.getElementById('table-container');
            if(!tableContainer) return;
            tableContainer.innerHTML = '';
            
            const charactersByRole = getCharactersByRole(tableData.bax.data, tableData.bal.data);
            
            tableContainer.innerHTML = createTableElement(tableData.bax.data, tableData.bax.colors, 'bax', charactersByRole) + 
                                       createTableElement(tableData.bal.data, tableData.bal.colors, 'bal', charactersByRole);
            
            addTableInteractivity(charactersByRole);
        };
        
        const createMobileColorPalette = (cell) => {
            let existingPalette = document.querySelector('.mobile-color-palette');
            if (existingPalette) {
                existingPalette.remove();
            }

            const palette = document.createElement('div');
            palette.className = 'mobile-color-palette';
            
            const colors = document.querySelectorAll('#color-palette .color-box');
            colors.forEach(colorBox => {
                const itemContainer = document.createElement('div');
                itemContainer.className = 'palette-item';
                itemContainer.dataset.color = colorBox.dataset.color;

                const newBox = document.createElement('div');
                newBox.className = 'color-box';
                newBox.style.backgroundColor = colorBox.dataset.color;
                
                const colorNameSpan = document.createElement('span');
                colorNameSpan.className = 'color-name';
                colorNameSpan.textContent = colorBox.title;

                itemContainer.appendChild(newBox);
                itemContainer.appendChild(colorNameSpan);
                
                itemContainer.addEventListener('click', (e) => {
                    e.stopPropagation();
                    applyColorToCell(cell, itemContainer.dataset.color);
                    palette.remove();
                });
                palette.appendChild(itemContainer);
            });
            
            // Adiciona a paleta ao body para calcular suas dimensões
            document.body.appendChild(palette);
            const paletteRect = palette.getBoundingClientRect();
            
            // Posicionar a paleta
            const rect = cell.getBoundingClientRect();
            let left = rect.left + window.scrollX;
            let top = rect.bottom + window.scrollY + 5; // Posição inicial abaixo da célula
            
            // Ajustar para não sair da tela
            if (left + paletteRect.width > window.innerWidth) {
                left = window.innerWidth - paletteRect.width - 10;
            }
             if (left < 10) {
                left = 10;
            }
            if (top + paletteRect.height > window.innerHeight) {
                top = rect.top + window.scrollY - paletteRect.height - 5;
            }
             if (top < window.scrollY + 10) {
                top = window.scrollY + 10;
            }
            
            palette.style.left = `${left}px`;
            palette.style.top = `${top}px`;
            
            // Remover a paleta ao clicar fora dela
            const removePalette = (e) => {
                if (!palette.contains(e.target) && e.target !== cell) {
                    palette.remove();
                    document.removeEventListener('click', removePalette, true);
                    document.removeEventListener('contextmenu', removePalette, true);
                }
            };
            // Usar 'true' para capturar o evento antes que outros listeners o façam
            setTimeout(() => {
                document.addEventListener('click', removePalette, true);
                document.addEventListener('contextmenu', removePalette, true);
            }, 100);
        };

        const addTableInteractivity = (charactersByRole) => {
             ['bax', 'bal'].forEach(tableType => {
                tableData[tableType].data.forEach((rowData, rowIndex) => {
                    ['Solador', 'Suporte 1', 'Suporte 2'].forEach((header, colIndexOffset) => {
                        const colIndex = colIndexOffset + 2;
                        const cellId = `${rowIndex}-${colIndex}-${tableType}`;
                        const cell = document.getElementById(cellId);
                        
                        if (!cell) return;
                        
                        // Evento para remover cor (funciona em desktop e mobile)
                        cell.ondblclick = () => {
                            delete tableData[tableType].colors[cellId];
                            cell.style.backgroundColor = '';
                            const input = cell.querySelector('input');
                            if (input) input.style.color = getComputedStyle(document.body).getPropertyValue('--text-primary');
                            updateAllTables();
                        };
                        
                        // Lógica de arrastar e soltar (apenas para desktop)
                        cell.ondragover = (event) => event.preventDefault();
                        cell.ondrop = (event) => {
                            event.preventDefault();
                            const color = event.dataTransfer.getData('text/plain');
                            const targetCell = event.target.closest('td');
                            if (targetCell) applyColorToCell(targetCell, color);
                        };

                        // Lógica de segurar para colorir (mobile) e clique direito (desktop)
                        let pressTimer;
                        cell.addEventListener('touchstart', (e) => {
                            pressTimer = window.setTimeout(() => {
                                e.preventDefault();
                                createMobileColorPalette(cell);
                            }, 500); // 500ms para um toque longo
                        });
                        cell.addEventListener('touchend', () => {
                            clearTimeout(pressTimer);
                        });
                        cell.addEventListener('touchmove', () => {
                            clearTimeout(pressTimer);
                        });


                        cell.addEventListener('contextmenu', (e) => {
                            e.preventDefault();
                            createMobileColorPalette(cell);
                        });

                        const dropdownContainer = document.getElementById(`dropdown-container-${cellId}`);
                        if (dropdownContainer) {
                            let optionsList = [];
                            const dayKey = `${tableType}-${rowData.Dia}`;
                            const customDayList = customLists[dayKey];
                            
                            if (customDayList) {
                                optionsList = header === 'Solador' ? customDayList.solador : customDayList.suporte;
                            } else {
                                optionsList = header === 'Solador' ? charactersByRole.soladors : charactersByRole.suportes;
                            }
                            
                            const dropdown = createSearchableDropdown(optionsList, rowData[header], (selected) => {
                                // Atualiza o dado diretamente na estrutura de dados
                                tableData[tableType].data[rowIndex][header] = selected;
                                updateAllTables();
                            }, cell);
                            dropdownContainer.appendChild(dropdown);
                        }
                    });
                });

                const clearButton = document.getElementById(`clear-${tableType}-button`);
                if (clearButton) {
                    clearButton.addEventListener('click', () => {
                        showConfirmModal(`Tem certeza de que deseja limpar todas as cores da tabela ${tableType.toUpperCase()}?`, () => {
                            tableData[tableType].colors = {};
                            updateAllTables();
                            saveProgress();
                            showMessage(`Cores de ${tableType.toUpperCase()} limpas.`, 'success');
                        });
                    });
                }
            });
        };

        const exportCombinedImage = () => {
            const nickname = document.getElementById('nickname-input').value;
            showMessage('Gerando imagem, aguarde...', 'info');
            
            const tempContainer = document.createElement('div');
            tempContainer.style.position = 'absolute';
            tempContainer.style.left = '-9999px';
            tempContainer.style.width = '1200px'; 
            tempContainer.style.padding = '40px'; 
            tempContainer.style.backgroundColor = getComputedStyle(document.body).getPropertyValue('--bg-primary');
            tempContainer.style.color = getComputedStyle(document.body).getPropertyValue('--text-primary');
            document.body.appendChild(tempContainer);

            const titleElement = document.createElement('h1');
            titleElement.textContent = `BAX e BAL de ${nickname || 'Player'}`;
            titleElement.className = 'text-4xl font-bold text-center mb-8';
            titleElement.style.color = getComputedStyle(document.body).getPropertyValue('--accent-teal');
            tempContainer.appendChild(titleElement);
            
            const charactersByRole = getCharactersByRole(tableData.bax.data, tableData.bal.data);
            const baxHtml = createTableElement(tableData.bax.data, tableData.bax.colors, 'bax', charactersByRole, true);
            const balHtml = createTableElement(tableData.bal.data, tableData.bal.colors, 'bal', charactersByRole, true);
            
            const tablesWrapper = document.createElement('div');
            tablesWrapper.className = 'space-y-10';
            tablesWrapper.innerHTML = baxHtml + balHtml;
            tempContainer.appendChild(tablesWrapper);

            html2canvas(tempContainer, { 
                useCORS: true, 
                backgroundColor: getComputedStyle(document.body).getPropertyValue('--bg-primary'),
                scale: 1.5
            }).then(canvas => {
                const link = document.createElement('a');
                link.href = canvas.toDataURL('image/png');
                link.download = `CTPS_${nickname || 'combinado'}.png`;
                link.click();
                showMessage('Imagem exportada com sucesso!', 'success');
            }).catch(e => {
                showMessage('Erro ao gerar imagem: ' + e.message, 'error');
            }).finally(() => {
                document.body.removeChild(tempContainer);
            });
        };
        
        const applyColorToCell = (cell, color) => {
            const { rowIndex, colIndex, tableType } = cell.dataset;
            const cellKey = `${rowIndex}-${colIndex}-${tableType}`;
            
            cell.style.backgroundColor = color;
            tableData[tableType].colors[cellKey] = color;
            
            const inputElement = cell.querySelector('input');
            if (inputElement) {
                saveProgress();
                inputElement.style.color = isColorLight(color) ? '#0f172a' : '#f8fafc';
            }
            updateAllTables();
        };

        const setupMobileMenu = () => {
            const menuButton = document.getElementById('mobile-menu-button');
            const menu = document.getElementById('color-palette');
            const overlay = document.getElementById('mobile-menu-overlay');

            const toggleMenu = () => {
                menu.classList.toggle('open');
                overlay.classList.toggle('hidden');
            };

            menuButton.addEventListener('click', toggleMenu);
            overlay.addEventListener('click', toggleMenu);
        };
        
        // Funções para Gerenciar Personagens
        let currentManageDay = '';

        const manageModal = document.getElementById('manage-characters-modal');
        const daySelect = document.getElementById('day-select');
        const addSoladorInput = document.getElementById('add-solador-input');
        const addSoladorBtn = document.getElementById('add-solador-btn');
        const soladorList = document.getElementById('solador-list');
        const addSuporteInput = document.getElementById('add-suporte-input');
        const addSuporteBtn = document.getElementById('add-suporte-btn');
        const suporteList = document.getElementById('suporte-list');
        const saveCharactersBtn = document.getElementById('save-characters-btn');
        const cancelManageBtn = document.getElementById('cancel-manage-btn');

        const populateManagementModal = (dayKey) => {
            currentManageDay = dayKey;
            soladorList.innerHTML = '';
            suporteList.innerHTML = '';

            const dayLists = customLists[dayKey] || { solador: [], suporte: [] };

            dayLists.solador.forEach(char => createCharacterItem(soladorList, char, 'solador'));
            dayLists.suporte.forEach(char => createCharacterItem(suporteList, char, 'suporte'));
        };

        const createCharacterItem = (listElement, charName, role) => {
            const li = document.createElement('li');
            li.className = 'flex justify-between items-center p-2 rounded-md';
            li.style.backgroundColor = 'var(--bg-card-hover)';
            li.innerHTML = `
                <span style="color: var(--text-primary);">${charName}</span>
                <button class="remove-char-btn text-[var(--accent-red)] hover:text-red-500 font-bold leading-none p-1 transition-colors duration-200" data-role="${role}" data-name="${charName}">
                    &times;
                </button>
            `;
            listElement.appendChild(li);
        };

        const handleAddCharacter = (role) => {
            const input = role === 'solador' ? addSoladorInput : addSuporteInput;
            const charName = input.value.trim();
            if (!charName) return;

            if (!customLists[currentManageDay]) {
                customLists[currentManageDay] = { solador: [], suporte: [] };
            }

            const list = customLists[currentManageDay][role];
            if (!list.includes(charName)) {
                list.push(charName);
                list.sort();
                populateManagementModal(currentManageDay);
                input.value = '';
                showMessage(`${charName} adicionado à lista.`, 'success');
            } else {
                showMessage(`${charName} já existe na lista.`, 'info');
            }
        };

        const handleRemoveCharacter = (role, charName) => {
            if (customLists[currentManageDay] && customLists[currentManageDay][role]) {
                const list = customLists[currentManageDay][role];
                const index = list.indexOf(charName);
                if (index > -1) {
                    list.splice(index, 1);
                    populateManagementModal(currentManageDay);
                    showMessage(`${charName} removido da lista.`, 'info');
                }
            }
        };

        // --- Funções de Ranking ---
        const calculateCharacterRanking = () => {
            const ranking = {};
            const tables = [tableData.bax.data, tableData.bal.data];
            tables.forEach(table => {
                table.forEach(row => {
                    ['Solador', 'Suporte 1', 'Suporte 2'].forEach(col => {
                        const char = row[col];
                        if (char && char.length > 0) {
                            ranking[char] = (ranking[char] || 0) + 1;
                        }
                    });
                });
            });
            return Object.entries(ranking).sort((a, b) => b[1] - a[1]);
        };

        const calculateColorRanking = () => {
            const ranking = {};
            const colors = { ...tableData.bax.colors, ...tableData.bal.colors };
            for (const key in colors) {
                if (colors[key] && colors[key].length > 0) {
                    const colorName = colorNames[colors[key]] || 'Cor desconhecida';
                    ranking[colorName] = (ranking[colorName] || 0) + 1;
                }
            }
            return Object.entries(ranking).sort((a, b) => b[1] - a[1]);
        };
        
        const createRankingTableHtml = (title, headers, data) => {
            let html = `
                <div class="rounded-lg shadow-md w-full md:w-1/2 border"
                     style="background-color: transparent; border-color: var(--border-primary);">
                    <div class="p-4 border-b" style="border-color: var(--border-primary);">
                        <h2 class="text-xl font-bold text-[var(--accent-teal)] text-center">${title}</h2>
                    </div>
                    <div class="p-4 max-h-96 overflow-y-auto">
                        <table class="w-full text-sm text-left" style="color: var(--text-primary);">
                            <thead class="text-xs uppercase" style="color: var(--accent-teal); background-color: var(--bg-table-header);">
                                <tr>
                                    <th scope="col" class="px-3 py-2">${headers[0]}</th>
                                    <th scope="col" class="px-3 py-2 text-right">${headers[1]}</th>
                                </tr>
                            </thead>
                            <tbody>
            `;
            if (data.length === 0) {
                html += `<tr><td colspan="2" class="px-3 py-3 text-center" style="color: var(--text-secondary);">Nenhum dado para exibir.</td></tr>`;
            } else {
                data.forEach(([name, count], index) => {
                    html += `
                        <tr class="border-b hover:bg-[var(--bg-card-hover)]" style="border-color: var(--border-secondary);">
                            <td class="px-3 py-2 font-medium whitespace-nowrap">${index + 1}. ${name}</td>
                            <td class="px-3 py-2 text-right">${count}</td>
                        </tr>
                    `;
                });
            }
            html += `
                            </tbody>
                        </table>
                    </div>
                </div>
            `;
            return html;
        };

        const renderRankingTables = () => {
            const rankingContainer = document.getElementById('ranking-tables-container');
             if(!rankingContainer) return;
            const characterRanking = calculateCharacterRanking();
            const colorRanking = calculateColorRanking();
            
            const characterTable = createRankingTableHtml('Ranking de Personagens', ['Personagem', 'Contagem'], characterRanking);
            const colorTable = createRankingTableHtml('Ranking de CTPs', ['CTP', 'Contagem'], colorRanking);
            
            rankingContainer.innerHTML = characterTable + colorTable;
        };
        
        const updateAllTables = () => {
            renderTables();
            renderRankingTables();
            saveProgress();
        };

        const applyTheme = (theme) => {
            const body = document.body;
            const themeIconPath = document.getElementById('theme-icon-path');

            if (theme === 'light') {
                body.classList.add('light-mode');
                themeIconPath.setAttribute('d', "M20.354 15.354A9 9 0 018.646 3.646 9.003 9.003 0 0012 21a9.003 9.003 0 008.354-5.646z");
            } else {
                body.classList.remove('light-mode');
                themeIconPath.setAttribute('d', "M12 3v1m0 16v1m9-9h-1M4 12H3m15.364 6.364l-.707-.707M6.343 6.343l-.707-.707m12.728 0l-.707.707M6.343 17.657l-.707.707M16 12a4 4 0 11-8 0 4 4 0 018 0z");
            }
            localStorage.setItem('theme', theme);
            updateAllTables(); // Re-render tables to apply new colors
        };


        // Inicialização do aplicativo
        document.addEventListener('DOMContentLoaded', () => {
            // Inicializa os dados da tabela apenas uma vez na carga da página
            tableData.bax.data = parseCsv(baxCsv).data;
            tableData.bal.data = parseCsv(balCsv).data;

            loadCustomLists();
            loadProgress();
            
            // Aplica o tema salvo ou o padrão
            const savedTheme = localStorage.getItem('theme') || 'dark';
            applyTheme(savedTheme);

            document.getElementById('theme-toggle').addEventListener('click', () => {
                const currentTheme = localStorage.getItem('theme') || 'dark';
                const newTheme = currentTheme === 'dark' ? 'light' : 'dark';
                applyTheme(newTheme);
            });

             document.getElementById('reset-selections-btn').addEventListener('click', () => {
                showConfirmModal('Tem certeza que deseja redefinir todas as seleções de personagens para o padrão?', () => {
                    tableData.bax.data = parseCsv(baxCsv).data;
                    tableData.bal.data = parseCsv(balCsv).data;
                    updateAllTables();
                    showMessage('Seleções de personagens redefinidas.', 'success');
                });
            });

            document.getElementById('export-combined-image-button').addEventListener('click', exportCombinedImage);

            document.getElementById('manual-save-button').addEventListener('click', () => {
                saveProgress();
                showMessage('Progresso salvo manualmente!', 'success');
            });

            document.getElementById('manage-characters-button').addEventListener('click', () => {
                manageModal.classList.remove('hidden');
                // Preencher o select do modal com todas as opções
                daySelect.innerHTML = '';
                tableData.bax.data.forEach(row => {
                    const option = document.createElement('option');
                    option.value = `bax-${row.Dia}`;
                    option.textContent = `BAX - Dia ${row.Dia}`;
                    daySelect.appendChild(option);
                });
                tableData.bal.data.forEach(row => {
                    const option = document.createElement('option');
                    option.value = `bal-${row.Dia}`;
                    option.textContent = `BAL - Dia ${row.Dia}`;
                    daySelect.appendChild(option);
                });
                
                // Popula a modal com o primeiro dia por padrão
                populateManagementModal(daySelect.value);
            });

            daySelect.addEventListener('change', (e) => {
                populateManagementModal(e.target.value);
            });
            addSoladorBtn.addEventListener('click', () => handleAddCharacter('solador'));
            addSuporteBtn.addEventListener('click', () => handleAddCharacter('suporte'));

            // Use delegação de eventos para os botões de remover
            soladorList.addEventListener('click', (e) => {
                if (e.target.classList.contains('remove-char-btn')) {
                    const role = e.target.dataset.role;
                    const charName = e.target.dataset.name;
                    handleRemoveCharacter(role, charName);
                }
            });
            suporteList.addEventListener('click', (e) => {
                if (e.target.classList.contains('remove-char-btn')) {
                    const role = e.target.dataset.role;
                    const charName = e.target.dataset.name;
                    handleRemoveCharacter(role, charName);
                }
            });

            saveCharactersBtn.addEventListener('click', () => {
                saveCustomLists();
                manageModal.classList.add('hidden');
                updateAllTables();
                showMessage('Listas de personagens salvas!', 'success');
            });
            cancelManageBtn.addEventListener('click', () => {
                manageModal.classList.add('hidden');
                loadCustomLists(); // Recarrega os dados para descartar alterações
            });

            // Adiciona a funcionalidade de clique para remover o atributo readonly
            const nicknameInput = document.getElementById('nickname-input');
            nicknameInput.addEventListener('click', () => {
                nicknameInput.removeAttribute('readonly');
                nicknameInput.focus();
            });

            nicknameInput.addEventListener('input', () => {
                saveProgress();
            });

            setupMobileMenu();
            
            // Lógica de arrastar e soltar para desktop, usando delegação de eventos
            if (window.innerWidth >= 768) {
                const colorPalette = document.getElementById('color-palette');
                if (colorPalette) {
                    colorPalette.addEventListener('dragstart', (event) => {
                        const colorBox = event.target.closest('.color-box');
                        if (colorBox && colorBox.draggable) {
                            event.dataTransfer.setData('text/plain', colorBox.dataset.color);
                            colorBox.classList.add('dragging');
                        }
                    });

                    colorPalette.addEventListener('dragend', (event) => {
                        const colorBox = event.target.closest('.color-box');
                        if (colorBox) {
                            colorBox.classList.remove('dragging');
                        }
                    });
                }
            }
        });
    </script>
</body>
</html>


