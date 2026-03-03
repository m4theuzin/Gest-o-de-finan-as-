<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta name="description" content="Sistema de Gestão de Empréstimos com Juros - Controle completo de clientes e pagamentos">
    <title>LoanManager Pro - Controle de Empréstimos</title>
    
    <!-- PWA Manifest -->
    <link rel="manifest" href="data:application/json;base64,eyJuYW1lIjoiTG9hbk1hbmFnZXIgUHJvIiwic2hvcnRfbmFtZSI6IkxvYW5NYW5hZ2VyIiwic3RhcnRfdXJsIjoiLiIsImRpc3BsYXkiOiJzdGFuZGFsb25lIiwiYmFja2dyb3VuZF9jb2xvciI6IiMxZTI5M2IiLCJ0aGVtZV9jb2xvciI6IiMxZTI5M2IiLCJpY29ucyI6W3sic3JjIjoiZGF0YTppbWFnZS9zdmcreG1sLCUzQ3N2ZyB4bWxucz0naHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmcnIHZpZXdCb3g9JzAgMCAxOTIgMTkyJyBmaWxsPSAnIzEwYjk4MSclM0UlM0NwYXRoIGQ9J00xNzYgODBhNTYgNTYgMCAxLTgwIDBsLTI0LTI0YTU2IDU2IDAgMSAxIDgwLTgwYzIyIDIyIDIyIDU4IDAgODBsLTI0IDI0YTU2IDU2IDAgMCAwIDgwIDgwYzIyLTIyIDIyLTU4IDAtODBsMjQgMjRhNTYgNTYgMCAwIDAgODAgODBjMjItMjIgMjItNTggMC04MCcvJTNFJTNDL3N2ZyUzRSIsInNpemVzIjoiMTkyeDE5MiIsInR5cGUiOiJpbWFnZS9zdmcreG1sIn1dfQ==">
    
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <style>
        * {
            font-family: 'Inter', sans-serif;
            -webkit-tap-highlight-color: transparent;
        }
        
        body {
            background: #0f172a;
            color: #e2e8f0;
            overflow-x: hidden;
        }
        
        .glass {
            background: rgba(30, 41, 59, 0.7);
            backdrop-filter: blur(12px);
            border: 1px solid rgba(255, 255, 255, 0.1);
        }
        
        .card-hover {
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
        }
        
        .card-hover:active {
            transform: scale(0.98);
        }
        
        .input-field {
            background: rgba(15, 23, 42, 0.8);
            border: 1px solid rgba(255, 255, 255, 0.1);
            transition: all 0.3s ease;
        }
        
        .input-field:focus {
            border-color: #10b981;
            box-shadow: 0 0 0 3px rgba(16, 185, 129, 0.1);
            outline: none;
        }
        
        .btn-primary {
            background: linear-gradient(135deg, #10b981 0%, #059669 100%);
            box-shadow: 0 4px 6px -1px rgba(16, 185, 129, 0.2);
        }
        
        .btn-primary:active {
            transform: translateY(1px);
        }
        
        .status-badge {
            font-size: 0.75rem;
            padding: 0.25rem 0.75rem;
            border-radius: 9999px;
            font-weight: 600;
        }
        
        .status-pending { background: rgba(245, 158, 11, 0.2); color: #fbbf24; }
        .status-paid { background: rgba(16, 185, 129, 0.2); color: #34d399; }
        .status-overdue { background: rgba(239, 68, 68, 0.2); color: #f87171; }
        
        .slide-up {
            animation: slideUp 0.3s ease-out;
        }
        
        @keyframes slideUp {
            from { transform: translateY(20px); opacity: 0; }
            to { transform: translateY(0); opacity: 1; }
        }
        
        .fade-in {
            animation: fadeIn 0.3s ease-out;
        }
        
        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }
        
        .fab {
            position: fixed;
            bottom: 24px;
            right: 24px;
            width: 56px;
            height: 56px;
            border-radius: 50%;
            background: linear-gradient(135deg, #10b981 0%, #059669 100%);
            display: flex;
            align-items: center;
            justify-content: center;
            box-shadow: 0 4px 12px rgba(16, 185, 129, 0.4);
            z-index: 40;
            transition: transform 0.2s;
        }
        
        .fab:active {
            transform: scale(0.9);
        }
        
        /* Custom Scrollbar */
        ::-webkit-scrollbar {
            width: 6px;
            height: 6px;
        }
        
        ::-webkit-scrollbar-track {
            background: rgba(255, 255, 255, 0.05);
        }
        
        ::-webkit-scrollbar-thumb {
            background: rgba(255, 255, 255, 0.2);
            border-radius: 3px;
        }
        
        .tab-active {
            color: #10b981;
            border-bottom: 2px solid #10b981;
        }
        
        .client-card {
            border-left: 4px solid transparent;
        }
        
        .client-card.pending { border-left-color: #fbbf24; }
        .client-card.paid { border-left-color: #34d399; }
        .client-card.overdue { border-left-color: #f87171; }
        
        .money-text {
            font-family: 'Courier New', monospace;
            letter-spacing: -0.5px;
        }
        
        /* Hide number input arrows */
        input[type=number]::-webkit-inner-spin-button, 
        input[type=number]::-webkit-outer-spin-button { 
            -webkit-appearance: none; 
            margin: 0; 
        }
    </style>
</head>
<body class="min-h-screen pb-20">

    <!-- Header -->
    <header class="fixed top-0 w-full z-50 glass border-b border-white/10">
        <div class="max-w-5xl mx-auto px-4 h-16 flex items-center justify-between">
            <div class="flex items-center gap-3">
                <div class="w-10 h-10 rounded-xl bg-gradient-to-br from-emerald-500 to-teal-600 flex items-center justify-center">
                    <i class="fas fa-hand-holding-dollar text-white text-lg"></i>
                </div>
                <div>
                    <h1 class="font-bold text-lg leading-tight">LoanManager</h1>
                    <p class="text-xs text-gray-400">Gestão de Empréstimos</p>
                </div>
            </div>
            
            <div class="flex items-center gap-3">
                <button onclick="exportData()" class="p-2 rounded-lg hover:bg-white/10 transition-colors" title="Exportar Dados">
                    <i class="fas fa-download text-gray-400"></i>
                </button>
                <button onclick="importData()" class="p-2 rounded-lg hover:bg-white/10 transition-colors" title="Importar Dados">
                    <i class="fas fa-upload text-gray-400"></i>
                </button>
                <input type="file" id="importFile" class="hidden" accept=".json" onchange="handleImport(event)">
            </div>
        </div>
    </header>

    <!-- Main Content -->
    <main class="pt-20 px-4 max-w-5xl mx-auto">
        
        <!-- Stats Cards -->
        <div class="grid grid-cols-3 gap-3 mb-6 mt-4">
            <div class="glass rounded-xl p-3 text-center">
                <p class="text-xs text-gray-400 mb-1">Total Emprestado</p>
                <p class="text-emerald-400 font-bold text-sm money-text" id="totalLoaned">R$ 0,00</p>
            </div>
            <div class="glass rounded-xl p-3 text-center">
                <p class="text-xs text-gray-400 mb-1">A Receber</p>
                <p class="text-amber-400 font-bold text-sm money-text" id="totalReceivable">R$ 0,00</p>
            </div>
            <div class="glass rounded-xl p-3 text-center">
                <p class="text-xs text-gray-400 mb-1">Clientes</p>
                <p class="text-blue-400 font-bold text-lg" id="totalClients">0</p>
            </div>
        </div>

        <!-- Search and Filter -->
        <div class="flex gap-2 mb-4">
            <div class="flex-1 relative">
                <i class="fas fa-search absolute left-3 top-1/2 transform -translate-y-1/2 text-gray-500 text-sm"></i>
                <input type="text" id="searchInput" placeholder="Buscar cliente..." 
                    class="input-field w-full pl-10 pr-4 py-2.5 rounded-xl text-sm text-white placeholder-gray-500"
                    oninput="filterClients()">
            </div>
            <select id="filterStatus" onchange="filterClients()" 
                class="input-field px-3 py-2.5 rounded-xl text-sm text-white min-w-[110px]">
                <option value="all">Todos</option>
                <option value="pending">Pendentes</option>
                <option value="overdue">Vencidos</option>
                <option value="paid">Pagos</option>
            </select>
        </div>

        <!-- Tabs -->
        <div class="flex border-b border-white/10 mb-4 overflow-x-auto">
            <button onclick="switchTab('active')" id="tab-active" class="tab-active flex-1 py-3 text-sm font-medium whitespace-nowrap transition-colors">
                Ativos
            </button>
            <button onclick="switchTab('paid')" id="tab-paid" class="flex-1 py-3 text-sm font-medium text-gray-400 whitespace-nowrap transition-colors hover:text-white">
                Pagos
            </button>
            <button onclick="switchTab('all')" id="tab-all" class="flex-1 py-3 text-sm font-medium text-gray-400 whitespace-nowrap transition-colors hover:text-white">
                Todos
            </button>
        </div>

        <!-- Clients List -->
        <div id="clientsList" class="space-y-3">
            <!-- Clients will be rendered here -->
        </div>

        <!-- Empty State -->
        <div id="emptyState" class="hidden text-center py-12">
            <div class="w-16 h-16 mx-auto mb-4 rounded-full bg-white/5 flex items-center justify-center">
                <i class="fas fa-users text-2xl text-gray-600"></i>
            </div>
            <p class="text-gray-400 text-sm">Nenhum cliente cadastrado</p>
            <p class="text-gray-500 text-xs mt-1">Clique no + para adicionar</p>
        </div>
    </main>

    <!-- Floating Action Button -->
    <button onclick="openModal()" class="fab">
        <i class="fas fa-plus text-white text-xl"></i>
    </button>

    <!-- Add/Edit Client Modal -->
    <div id="clientModal" class="fixed inset-0 z-50 hidden">
        <div class="absolute inset-0 bg-black/80 backdrop-blur-sm" onclick="closeModal()"></div>
        <div class="absolute bottom-0 left-0 right-0 md:top-1/2 md:left-1/2 md:bottom-auto md:right-auto md:transform md:-translate-x-1/2 md:-translate-y-1/2 md:w-full md:max-w-lg bg-slate-900 rounded-t-3xl md:rounded-2xl max-h-[90vh] overflow-y-auto slide-up">
            
            <div class="sticky top-0 bg-slate-900 p-4 border-b border-white/10 flex items-center justify-between z-10">
                <h2 class="text-lg font-bold" id="modalTitle">Novo Empréstimo</h2>
                <button onclick="closeModal()" class="w-8 h-8 rounded-full bg-white/10 flex items-center justify-center">
                    <i class="fas fa-times text-sm"></i>
                </button>
            </div>

            <form id="clientForm" onsubmit="event.preventDefault(); saveClient();" class="p-4 space-y-4">
                <input type="hidden" id="clientId">
                
                <!-- Personal Info -->
                <div class="space-y-3">
                    <h3 class="text-xs font-semibold text-gray-400 uppercase tracking-wider">Dados Pessoais</h3>
                    
                    <div>
                        <label class="block text-xs text-gray-400 mb-1">Nome Completo *</label>
                        <input type="text" id="clientName" required class="input-field w-full px-4 py-3 rounded-xl text-white text-sm" placeholder="Ex: João Silva">
                    </div>

                    <div class="grid grid-cols-2 gap-3">
                        <div>
                            <label class="block text-xs text-gray-400 mb-1">CPF *</label>
                            <input type="text" id="clientCPF" required maxlength="14" class="input-field w-full px-4 py-3 rounded-xl text-white text-sm" placeholder="000.000.000-00" oninput="formatCPF(this)">
                        </div>
                        <div>
                            <label class="block text-xs text-gray-400 mb-1">Data de Nascimento</label>
                            <input type="date" id="clientBirth" class="input-field w-full px-4 py-3 rounded-xl text-white text-sm">
                        </div>
                    </div>

                    <div class="grid grid-cols-2 gap-3">
                        <div>
                            <label class="block text-xs text-gray-400 mb-1">Telefone *</label>
                            <input type="tel" id="clientPhone" required class="input-field w-full px-4 py-3 rounded-xl text-white text-sm" placeholder="(00) 00000-0000" oninput="formatPhone(this)">
                        </div>
                        <div>
                            <label class="block text-xs text-gray-400 mb-1">CEP</label>
                            <input type="text" id="clientCEP" maxlength="9" class="input-field w-full px-4 py-3 rounded-xl text-white text-sm" placeholder="00000-000" oninput="formatCEP(this)" onblur="fetchAddress(this.value)">
                        </div>
                    </div>

                    <div id="addressFields" class="hidden space-y-3">
                        <input type="text" id="clientStreet" class="input-field w-full px-4 py-3 rounded-xl text-white text-sm" placeholder="Rua/Avenida">
                        <div class="grid grid-cols-2 gap-3">
                            <input type="text" id="clientNumber" class="input-field w-full px-4 py-3 rounded-xl text-white text-sm" placeholder="Número">
                            <input type="text" id="clientNeighborhood" class="input-field w-full px-4 py-3 rounded-xl text-white text-sm" placeholder="Bairro">
                        </div>
                        <div class="grid grid-cols-2 gap-3">
                            <input type="text" id="clientCity" class="input-field w-full px-4 py-3 rounded-xl text-white text-sm" placeholder="Cidade">
                            <input type="text" id="clientState" class="input-field w-full px-4 py-3 rounded-xl text-white text-sm" placeholder="UF" maxlength="2">
                        </div>
                    </div>
                </div>

                <!-- Loan Info -->
                <div class="space-y-3 pt-2 border-t border-white/10">
                    <h3 class="text-xs font-semibold text-gray-400 uppercase tracking-wider">Dados do Empréstimo</h3>
                    
                    <div class="grid grid-cols-2 gap-3">
                        <div>
                            <label class="block text-xs text-gray-400 mb-1">Valor Emprestado (R$) *</label>
                            <input type="number" id="loanAmount" required step="0.01" min="0" class="input-field w-full px-4 py-3 rounded-xl text-white text-sm money-text" placeholder="0,00" oninput="calculateTotal()">
                        </div>
                        <div>
                            <label class="block text-xs text-gray-400 mb-1">Juros (% ao mês)</label>
                            <input type="number" id="interestRate" step="0.01" min="0" class="input-field w-full px-4 py-3 rounded-xl text-white text-sm" placeholder="0%" value="0" oninput="calculateTotal()">
                        </div>
                    </div>

                    <div class="grid grid-cols-2 gap-3">
                        <div>
                            <label class="block text-xs text-gray-400 mb-1">Data do Empréstimo *</label>
                            <input type="date" id="loanDate" required class="input-field w-full px-4 py-3 rounded-xl text-white text-sm" onchange="calculateDueDate()">
                        </div>
                        <div>
                            <label class="block text-xs text-gray-400 mb-1">Prazo (dias) *</label>
                            <input type="number" id="loanTerm" required min="1" class="input-field w-full px-4 py-3 rounded-xl text-white text-sm" placeholder="30" value="30" oninput="calculateDueDate()">
                        </div>
                    </div>

                    <div>
                        <label class="block text-xs text-gray-400 mb-1">Data de Vencimento *</label>
                        <input type="date" id="dueDate" required class="input-field w-full px-4 py-3 rounded-xl text-white text-sm">
                    </div>

                    <div class="glass rounded-xl p-4 bg-emerald-500/10 border border-emerald-500/20">
                        <div class="flex justify-between items-center mb-2">
                            <span class="text-xs text-gray-400">Total a Receber:</span>
                            <span class="text-lg font-bold text-emerald-400 money-text" id="totalAmount">R$ 0,00</span>
                        </div>
                        <div class="flex justify-between text-xs">
                            <span class="text-gray-500">Juros:</span>
                            <span class="text-emerald-300 money-text" id="interestAmount">R$ 0,00</span>
                        </div>
                    </div>
                </div>

                <div class="flex gap-3 pt-2">
                    <button type="button" onclick="closeModal()" class="flex-1 py-3 bg-white/10 rounded-xl text-sm font-medium hover:bg-white/20 transition-colors">
                        Cancelar
                    </button>
                    <button type="submit" class="flex-1 py-3 btn-primary rounded-xl text-sm font-medium text-white">
                        Salvar
                    </button>
                </div>
            </form>
        </div>
    </div>

    <!-- Client Detail Modal -->
    <div id="detailModal" class="fixed inset-0 z-50 hidden">
        <div class="absolute inset-0 bg-black/80 backdrop-blur-sm" onclick="closeDetailModal()"></div>
        <div class="absolute bottom-0 left-0 right-0 md:top-1/2 md:left-1/2 md:bottom-auto md:right-auto md:transform md:-translate-x-1/2 md:-translate-y-1/2 md:w-full md:max-w-lg bg-slate-900 rounded-t-3xl md:rounded-2xl max-h-[90vh] overflow-y-auto slide-up">
            
            <div class="p-4 border-b border-white/10 flex items-center justify-between sticky top-0 bg-slate-900 z-10">
                <h2 class="text-lg font-bold">Detalhes do Cliente</h2>
                <button onclick="closeDetailModal()" class="w-8 h-8 rounded-full bg-white/10 flex items-center justify-center">
                    <i class="fas fa-times text-sm"></i>
                </button>
            </div>

            <div id="detailContent" class="p-4 space-y-4">
                <!-- Content injected via JS -->
            </div>
        </div>
    </div>

    <!-- Install Prompt (PWA) -->
    <div id="installPrompt" class="fixed bottom-20 left-4 right-4 glass rounded-xl p-4 hidden z-40 border border-emerald-500/30">
        <div class="flex items-center justify-between">
            <div class="flex items-center gap-3">
                <div class="w-10 h-10 rounded-lg bg-emerald-500/20 flex items-center justify-center">
                    <i class="fas fa-download text-emerald-400"></i>
                </div>
                <div>
                    <p class="font-semibold text-sm">Instalar Aplicativo</p>
                    <p class="text-xs text-gray-400">Adicione à tela inicial</p>
                </div>
            </div>
            <div class="flex gap-2">
                <button onclick="dismissInstall()" class="px-3 py-1.5 text-xs text-gray-400 hover:text-white">Depois</button>
                <button onclick="installApp()" class="px-3 py-1.5 bg-emerald-500 rounded-lg text-xs font-medium text-white">Instalar</button>
            </div>
        </div>
    </div>

    <script>
        // Data Management
        let clients = JSON.parse(localStorage.getItem('loanClients')) || [];
        let currentTab = 'active';
        let deferredPrompt = null;

        // Initialize
        document.addEventListener('DOMContentLoaded', () => {
            renderClients();
            updateStats();
            
            // Set today's date as default
            document.getElementById('loanDate').valueAsDate = new Date();
            calculateDueDate();
            
            // PWA Install Prompt
            window.addEventListener('beforeinstallprompt', (e) => {
                deferredPrompt = e;
                setTimeout(() => {
                    if (!localStorage.getItem('installDismissed')) {
                        document.getElementById('installPrompt').classList.remove('hidden');
                    }
                }, 2000);
            });
        });

        // Formatters
        function formatCurrency(value) {
            return new Intl.NumberFormat('pt-BR', {
                style: 'currency',
                currency: 'BRL'
            }).format(value);
        }

        function formatDate(dateString) {
            if (!dateString) return '-';
            const date = new Date(dateString);
            return date.toLocaleDateString('pt-BR');
        }

        function formatCPF(input) {
            let value = input.value.replace(/\D/g, '');
            value = value.replace(/(\d{3})(\d)/, '$1.$2');
            value = value.replace(/(\d{3})(\d)/, '$1.$2');
            value = value.replace(/(\d{3})(\d{1,2})$/, '$1-$2');
            input.value = value;
        }

        function formatPhone(input) {
            let value = input.value.replace(/\D/g, '');
            if (value.length > 11) value = value.slice(0, 11);
            
            if (value.length > 10) {
                value = value.replace(/(\d{2})(\d{5})(\d{4})/, '($1) $2-$3');
            } else if (value.length > 6) {
                value = value.replace(/(\d{2})(\d{4})(\d{0,4})/, '($1) $2-$3');
            } else if (value.length > 2) {
                value = value.replace(/(\d{2})(\d{0,5})/, '($1) $2');
            }
            input.value = value;
        }

        function formatCEP(input) {
            let value = input.value.replace(/\D/g, '');
            if (value.length > 8) value = value.slice(0, 8);
            value = value.replace(/(\d{5})(\d)/, '$1-$2');
            input.value = value;
        }

        async function fetchAddress(cep) {
            const cleanCEP = cep.replace(/\D/g, '');
            if (cleanCEP.length !== 8) return;
            
            try {
                const response = await fetch(`https://viacep.com.br/ws/${cleanCEP}/json/`);
                const data = await response.json();
                
                if (!data.erro) {
                    document.getElementById('clientStreet').value = data.logradouro || '';
                    document.getElementById('clientNeighborhood').value = data.bairro || '';
                    document.getElementById('clientCity').value = data.localidade || '';
                    document.getElementById('clientState').value = data.uf || '';
                    document.getElementById('addressFields').classList.remove('hidden');
                }
            } catch (e) {
                console.error('Error fetching address:', e);
            }
        }

        // Calculations
        function calculateDueDate() {
            const loanDate = document.getElementById('loanDate').value;
            const term = parseInt(document.getElementById('loanTerm').value) || 30;
            
            if (loanDate) {
                const date = new Date(loanDate);
                date.setDate(date.getDate() + term);
                document.getElementById('dueDate').valueAsDate = date;
            }
            calculateTotal();
        }

        function calculateTotal() {
            const amount = parseFloat(document.getElementById('loanAmount').value) || 0;
            const rate = parseFloat(document.getElementById('interestRate').value) || 0;
            const term = parseInt(document.getElementById('loanTerm').value) || 30;
            
            // Simple interest calculation for the period
            const months = term / 30;
            const interest = amount * (rate / 100) * months;
            const total = amount + interest;
            
            document.getElementById('totalAmount').textContent = formatCurrency(total);
            document.getElementById('interestAmount').textContent = formatCurrency(interest);
        }

        // CRUD Operations
        function saveClient() {
            const id = document.getElementById('clientId').value;
            const client = {
                id: id || Date.now().toString(),
                name: document.getElementById('clientName').value,
                cpf: document.getElementById('clientCPF').value,
                birthDate: document.getElementById('clientBirth').value,
                phone: document.getElementById('clientPhone').value,
                cep: document.getElementById('clientCEP').value,
                address: {
                    street: document.getElementById('clientStreet').value,
                    number: document.getElementById('clientNumber').value,
                    neighborhood: document.getElementById('clientNeighborhood').value,
                    city: document.getElementById('clientCity').value,
                    state: document.getElementById('clientState').value
                },
                loanAmount: parseFloat(document.getElementById('loanAmount').value) || 0,
                interestRate: parseFloat(document.getElementById('interestRate').value) || 0,
                loanDate: document.getElementById('loanDate').value,
                termDays: parseInt(document.getElementById('loanTerm').value) || 30,
                dueDate: document.getElementById('dueDate').value,
                totalAmount: parseFloat(document.getElementById('totalAmount').textContent.replace(/[^\d,-]/g, '').replace(',', '.')) || 0,
                status: 'pending',
                createdAt: new Date().toISOString()
            };

            if (id) {
                const index = clients.findIndex(c => c.id === id);
                if (index !== -1) {
                    client.status = clients[index].status;
                    client.paymentDate = clients[index].paymentDate;
                    clients[index] = client;
                }
            } else {
                clients.push(client);
            }

            localStorage.setItem('loanClients', JSON.stringify(clients));
            closeModal();
            renderClients();
            updateStats();
            
            // Show success feedback
            showToast(id ? 'Cliente atualizado!' : 'Empréstimo cadastrado!');
        }

        function deleteClient(id) {
            if (confirm('Tem certeza que deseja excluir este cliente?')) {
                clients = clients.filter(c => c.id !== id);
                localStorage.setItem('loanClients', JSON.stringify(clients));
                renderClients();
                updateStats();
                closeDetailModal();
                showToast('Cliente removido');
            }
        }

        function markAsPaid(id) {
            const client = clients.find(c => c.id === id);
            if (client) {
                client.status = 'paid';
                client.paymentDate = new Date().toISOString();
                localStorage.setItem('loanClients', JSON.stringify(clients));
                renderClients();
                updateStats();
                closeDetailModal();
                showToast('Pagamento confirmado!');
            }
        }

        function markAsPending(id) {
            const client = clients.find(c => c.id === id);
            if (client) {
                client.status = 'pending';
                delete client.paymentDate;
                localStorage.setItem('loanClients', JSON.stringify(clients));
                renderClients();
                updateStats();
                closeDetailModal();
                showToast('Status atualizado para pendente');
            }
        }

        // Rendering
        function getStatus(client) {
            if (client.status === 'paid') return 'paid';
            const today = new Date();
            const due = new Date(client.dueDate);
            return today > due ? 'overdue' : 'pending';
        }

        function renderClients() {
            const list = document.getElementById('clientsList');
            const search = document.getElementById('searchInput').value.toLowerCase();
            const statusFilter = document.getElementById('filterStatus').value;
            
            let filtered = clients.filter(client => {
                const matchesSearch = client.name.toLowerCase().includes(search) || 
                                    client.cpf.includes(search) ||
                                    client.phone.includes(search);
                
                const clientStatus = getStatus(client);
                const matchesStatus = statusFilter === 'all' || 
                                    (statusFilter === 'overdue' && clientStatus === 'overdue') ||
                                    (statusFilter === 'pending' && clientStatus === 'pending' && client.status !== 'paid') ||
                                    (statusFilter === 'paid' && client.status === 'paid');
                
                const matchesTab = currentTab === 'all' || 
                                 (currentTab === 'paid' && client.status === 'paid') ||
                                 (currentTab === 'active' && client.status !== 'paid');
                
                return matchesSearch && matchesStatus && matchesTab;
            });

            // Sort by due date (closest first)
            filtered.sort((a, b) => new Date(a.dueDate) - new Date(b.dueDate));

            if (filtered.length === 0) {
                list.innerHTML = '';
                document.getElementById('emptyState').classList.remove('hidden');
                return;
            }

            document.getElementById('emptyState').classList.add('hidden');
            
            list.innerHTML = filtered.map(client => {
                const status = getStatus(client);
                const statusClass = status === 'paid' ? 'status-paid' : status === 'overdue' ? 'status-overdue' : 'status-pending';
                const statusText = status === 'paid' ? 'Pago' : status === 'overdue' ? 'Vencido' : 'Pendente';
                const daysLeft = Math.ceil((new Date(client.dueDate) - new Date()) / (1000 * 60 * 60 * 24));
                const daysText = status === 'paid' ? 'Pago em ' + formatDate(client.paymentDate) : 
                                daysLeft < 0 ? `${Math.abs(daysLeft)} dias atrasado` : 
                                daysLeft === 0 ? 'Vence hoje' : `${daysLeft} dias restantes`;

                return `
                    <div onclick="openDetailModal('${client.id}')" class="client-card ${status} glass rounded-xl p-4 card-hover cursor-pointer fade-in">
                        <div class="flex justify-between items-start mb-2">
                            <div class="flex-1 min-w-0">
                                <h3 class="font-semibold text-white truncate">${client.name}</h3>
                                <p class="text-xs text-gray-400 mt-0.5">${client.phone}</p>
                            </div>
                            <span class="status-badge ${statusClass} ml-2">${statusText}</span>
                        </div>
                        
                        <div class="flex justify-between items-end mt-3">
                            <div>
                                <p class="text-xs text-gray-500 mb-0.5">Valor Total</p>
                                <p class="text-emerald-400 font-bold money-text">${formatCurrency(client.totalAmount)}</p>
                            </div>
                            <div class="text-right">
                                <p class="text-xs ${daysLeft < 0 && status !== 'paid' ? 'text-red-400' : 'text-gray-500'} mb-0.5">
                                    ${daysText}
                                </p>
                                <p class="text-xs text-gray-400">Venc: ${formatDate(client.dueDate)}</p>
                            </div>
                        </div>
                    </div>
                `;
            }).join('');
        }

        function updateStats() {
            const totalLoaned = clients.reduce((sum, c) => sum + c.loanAmount, 0);
            const totalReceivable = clients
                .filter(c => c.status !== 'paid')
                .reduce((sum, c) => sum + c.totalAmount, 0);
            
            document.getElementById('totalLoaned').textContent = formatCurrency(totalLoaned);
            document.getElementById('totalReceivable').textContent = formatCurrency(totalReceivable);
            document.getElementById('totalClients').textContent = clients.length;
        }

        function filterClients() {
            renderClients();
        }

        function switchTab(tab) {
            currentTab = tab;
            
            // Update UI
            ['active', 'paid', 'all'].forEach(t => {
                const btn = document.getElementById(`tab-${t}`);
                if (t === tab) {
                    btn.classList.add('tab-active');
                    btn.classList.remove('text-gray-400');
                } else {
                    btn.classList.remove('tab-active');
                    btn.classList.add('text-gray-400');
                }
            });
            
            renderClients();
        }

        // Modal Management
        function openModal() {
            document.getElementById('clientForm').reset();
            document.getElementById('clientId').value = '';
            document.getElementById('modalTitle').textContent = 'Novo Empréstimo';
            document.getElementById('addressFields').classList.add('hidden');
            document.getElementById('loanDate').valueAsDate = new Date();
            calculateDueDate();
            document.getElementById('clientModal').classList.remove('hidden');
        }

        function openEditModal(id) {
            const client = clients.find(c => c.id === id);
            if (!client) return;

            document.getElementById('clientId').value = client.id;
            document.getElementById('clientName').value = client.name;
            document.getElementById('clientCPF').value = client.cpf;
            document.getElementById('clientBirth').value = client.birthDate;
            document.getElementById('clientPhone').value = client.phone;
            document.getElementById('clientCEP').value = client.cep || '';
            document.getElementById('clientStreet').value = client.address?.street || '';
            document.getElementById('clientNumber').value = client.address?.number || '';
            document.getElementById('clientNeighborhood').value = client.address?.neighborhood || '';
            document.getElementById('clientCity').value = client.address?.city || '';
            document.getElementById('clientState').value = client.address?.state || '';
            
            if (client.address?.street) {
                document.getElementById('addressFields').classList.remove('hidden');
            }

            document.getElementById('loanAmount').value = client.loanAmount;
            document.getElementById('interestRate').value = client.interestRate;
            document.getElementById('loanDate').value = client.loanDate;
            document.getElementById('loanTerm').value = client.termDays;
            document.getElementById('dueDate').value = client.dueDate;
            
            document.getElementById('modalTitle').textContent = 'Editar Empréstimo';
            calculateTotal();
            document.getElementById('clientModal').classList.remove('hidden');
            closeDetailModal();
        }

        function closeModal() {
            document.getElementById('clientModal').classList.add('hidden');
        }

        function openDetailModal(id) {
            const client = clients.find(c => c.id === id);
            if (!client) return;

            const status = getStatus(client);
            const statusClass = status === 'paid' ? 'status-paid' : status === 'overdue' ? 'status-overdue' : 'status-pending';
            const statusText = status === 'paid' ? 'Pago' : status === 'overdue' ? 'Vencido' : 'Pendente';

            const content = document.getElementById('detailContent');
            content.innerHTML = `
                <div class="flex items-center justify-between mb-6">
                    <div class="flex items-center gap-3">
                        <div class="w-12 h-12 rounded-full bg-gradient-to-br from-emerald-500 to-teal-600 flex items-center justify-center text-xl font-bold">
                            ${client.name.charAt(0).toUpperCase()}
                        </div>
                        <div>
                            <h3 class="font-bold text-lg">${client.name}</h3>
                            <span class="status-badge ${statusClass}">${statusText}</span>
                        </div>
                    </div>
                </div>

                <div class="space-y-4">
                    <div class="glass rounded-xl p-4">
                        <h4 class="text-xs font-semibold text-gray-400 uppercase mb-3">Dados Pessoais</h4>
                        <div class="space-y-2 text-sm">
                            <div class="flex justify-between">
                                <span class="text-gray-500">CPF:</span>
                                <span class="text-white font-mono">${client.cpf}</span>
                            </div>
                            ${client.birthDate ? `
                            <div class="flex justify-between">
                                <span class="text-gray-500">Nascimento:</span>
                                <span class="text-white">${formatDate(client.birthDate)}</span>
                            </div>` : ''}
                            <div class="flex justify-between">
                                <span class="text-gray-500">Telefone:</span>
                                <a href="tel:${client.phone}" class="text-emerald-400 hover:underline">${client.phone}</a>
                            </div>
                            ${client.cep ? `
                            <div class="flex justify-between">
                                <span class="text-gray-500">CEP:</span>
                                <span class="text-white font-mono">${client.cep}</span>
                            </div>` : ''}
                            ${client.address?.street ? `
                            <div class="pt-2 border-t border-white/10">
                                <p class="text-gray-500 text-xs mb-1">Endereço:</p>
                                <p class="text-white">${client.address.street}${client.address.number ? ', ' + client.address.number : ''}</p>
                                <p class="text-gray-400 text-xs">${client.address.neighborhood}, ${client.address.city} - ${client.address.state}</p>
                            </div>` : ''}
                        </div>
                    </div>

                    <div class="glass rounded-xl p-4 border-emerald-500/20 bg-emerald-500/5">
                        <h4 class="text-xs font-semibold text-emerald-400 uppercase mb-3">Dados do Empréstimo</h4>
                        <div class="space-y-3">
                            <div class="flex justify-between items-center">
                                <span class="text-gray-400 text-sm">Valor Emprestado:</span>
                                <span class="text-white font-bold money-text">${formatCurrency(client.loanAmount)}</span>
                            </div>
                            <div class="flex justify-between items-center">
                                <span class="text-gray-400 text-sm">Taxa de Juros:</span>
                                <span class="text-white">${client.interestRate}% ao mês</span>
                            </div>
                            <div class="flex justify-between items-center">
                                <span class="text-gray-400 text-sm">Data do Empréstimo:</span>
                                <span class="text-white">${formatDate(client.loanDate)}</span>
                            </div>
                            <div class="flex justify-between items-center">
                                <span class="text-gray-400 text-sm">Data de Vencimento:</span>
                                <span class="${status === 'overdue' ? 'text-red-400' : 'text-white'} font-medium">${formatDate(client.dueDate)}</span>
                            </div>
                            <div class="pt-3 border-t border-white/10 flex justify-between items-center">
                                <span class="text-gray-300 font-medium">Total a Receber:</span>
                                <span class="text-emerald-400 text-xl font-bold money-text">${formatCurrency(client.totalAmount)}</span>
                            </div>
                            ${client.status === 'paid' ? `
                            <div class="pt-2 border-t border-white/10 flex justify-between items-center">
                                <span class="text-gray-400 text-sm">Pago em:</span>
                                <span class="text-emerald-400">${formatDate(client.paymentDate)}</span>
                            </div>` : ''}
                        </div>
                    </div>

                    <div class="flex gap-2 pt-2">
                        ${client.status !== 'paid' ? `
                        <button onclick="markAsPaid('${client.id}')" class="flex-1 py-3 bg-emerald-500 hover:bg-emerald-600 rounded-xl text-sm font-medium text-white transition-colors flex items-center justify-center gap-2">
                            <i class="fas fa-check"></i> Marcar como Pago
                        </button>` : `
                        <button onclick="markAsPending('${client.id}')" class="flex-1 py-3 bg-amber-500 hover:bg-amber-600 rounded-xl text-sm font-medium text-white transition-colors flex items-center justify-center gap-2">
                            <i class="fas fa-undo"></i> Reabrir
                        </button>`}
                        <button onclick="openEditModal('${client.id}')" class="px-4 py-3 bg-white/10 hover:bg-white/20 rounded-xl text-white transition-colors">
                            <i class="fas fa-edit"></i>
                        </button>
                        <button onclick="deleteClient('${client.id}')" class="px-4 py-3 bg-red-500/20 hover:bg-red-500/30 text-red-400 rounded-xl transition-colors">
                            <i class="fas fa-trash"></i>
                        </button>
                    </div>
                </div>
            `;
            
            document.getElementById('detailModal').classList.remove('hidden');
        }

        function closeDetailModal() {
            document.getElementById('detailModal').classList.add('hidden');
        }

        // Data Export/Import
        function exportData() {
            const dataStr = JSON.stringify(clients, null, 2);
            const blob = new Blob([dataStr], { type: 'application/json' });
            const url = URL.createObjectURL(blob);
            const a = document.createElement('a');
            a.href = url;
            a.download = `loan-manager-backup-${new Date().toISOString().split('T')[0]}.json`;
            a.click();
            URL.revokeObjectURL(url);
            showToast('Dados exportados!');
        }

        function importData() {
            document.getElementById('importFile').click();
        }

        function handleImport(event) {
            const file = event.target.files[0];
            if (!file) return;

            const reader = new FileReader();
            reader.onload = (e) => {
                try {
                    const imported = JSON.parse(e.target.result);
                    if (Array.isArray(imported)) {
                        if (confirm(`Importar ${imported.length} registros? Isso substituirá os dados atuais.`)) {
                            clients = imported;
                            localStorage.setItem('loanClients', JSON.stringify(clients));
                            renderClients();
                            updateStats();
                            showToast('Dados importados com sucesso!');
                        }
                    } else {
                        alert('Arquivo inválido!');
                    }
                } catch (err) {
                    alert('Erro ao ler arquivo!');
                }
            };
            reader.readAsText(file);
            event.target.value = '';
        }

        // PWA Install
        function installApp() {
            if (deferredPrompt) {
                deferredPrompt.prompt();
                deferredPrompt.userChoice.then((choiceResult) => {
                    if (choiceResult.outcome === 'accepted') {
                        console.log('User accepted the install prompt');
                    }
                    deferredPrompt = null;
                    document.getElementById('installPrompt').classList.add('hidden');
                });
            }
        }

        function dismissInstall() {
            localStorage.setItem('installDismissed', 'true');
            document.getElementById('installPrompt').classList.add('hidden');
        }

        // Toast Notification
        function showToast(message) {
            const toast = document.createElement('div');
            toast.className = 'fixed bottom-24 left-1/2 transform -translate-x-1/2 bg-emerald-500 text-white px-6 py-3 rounded-full shadow-lg z-50 text-sm font-medium fade-in';
            toast.textContent = message;
            document.body.appendChild(toast);
            setTimeout(() => {
                toast.style.opacity = '0';
                toast.style.transition = 'opacity 0.3s';
                setTimeout(() => toast.remove(), 300);
            }, 2000);
        }

        // Service Worker Registration for PWA
        if ('serviceWorker' in navigator) {
            navigator.serviceWorker.register('data:text/javascript;base64,' + btoa(`
                self.addEventListener('install', e => self.skipWaiting());
                self.addEventListener('activate', e => e.waitUntil(clients.claim()));
                self.addEventListener('fetch', e => e.respondWith(fetch(e.request).catch(() => new Response('Offline'))));
            `)).catch(() => console.log('SW registration skipped'));
        }
    </script>
</body>
</html>

