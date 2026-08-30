# tic-tac-toe-2-player
```html
<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Tic Tac Toe - 2 Pemain</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Font Awesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        /* Animasi sederhana untuk kemunculan simbol X dan O */
        .symbol-pop {
            animation: popIn 0.25s cubic-bezier(0.175, 0.885, 0.32, 1.275) forwards;
        }

        @keyframes popIn {
            0% {
                transform: scale(0);
                opacity: 0;
            }
            100% {
                transform: scale(1);
                opacity: 1;
            }
        }
    </style>
</head>
<body class="bg-slate-900 text-slate-100 min-h-screen flex flex-col items-center justify-between p-4 font-sans select-none">

    <!-- Header & Judul -->
    <header class="text-center mt-4">
        <h1 class="text-3xl md:text-5xl font-extrabold tracking-wider bg-clip-text text-transparent bg-gradient-to-r from-cyan-400 to-indigo-500">
            TIC TAC TOE
        </h1>
        <p class="text-slate-400 text-sm md:text-base mt-1">Mode 2 Pemain (Bergantian)</p>
    </header>

    <!-- Main Container -->
    <main class="flex flex-col items-center justify-center w-full max-w-md my-auto gap-6">

        <!-- Papan Skor -->
        <div class="grid grid-cols-3 w-full bg-slate-800/80 backdrop-blur border border-slate-700 rounded-2xl p-4 text-center shadow-lg">
            <!-- Pemain X -->
            <div id="card-x" class="flex flex-col items-center p-2 rounded-xl transition-all duration-300 bg-indigo-500/20 border-2 border-indigo-500">
                <span class="text-xs font-semibold uppercase text-indigo-400">Pemain X</span>
                <span id="score-x" class="text-2xl font-bold text-indigo-400 mt-1">0</span>
            </div>

            <!-- Seri -->
            <div class="flex flex-col items-center justify-center p-2">
                <span class="text-xs font-semibold uppercase text-slate-400">Seri</span>
                <span id="score-draw" class="text-2xl font-bold text-slate-300 mt-1">0</span>
            </div>

            <!-- Pemain O -->
            <div id="card-o" class="flex flex-col items-center p-2 rounded-xl transition-all duration-300 border-2 border-transparent">
                <span class="text-xs font-semibold uppercase text-rose-400">Pemain O</span>
                <span id="score-o" class="text-2xl font-bold text-rose-400 mt-1">0</span>
            </div>
        </div>

        <!-- Status Giliran / Informasi Game -->
        <div id="status-display" class="text-lg md:text-xl font-bold text-slate-200 flex items-center gap-2">
            Giliran: <span id="current-turn" class="text-indigo-400 flex items-center gap-1"><i class="fa-solid fa-xmark text-2xl"></i> (X)</span>
        </div>

        <!-- Area Papan Game 3x3 -->
        <div id="board" class="grid grid-cols-3 gap-3 w-full aspect-square bg-slate-800/50 p-3 rounded-2xl border border-slate-700/50 shadow-2xl">
            <!-- 9 Sel Kotak Tic Tac Toe -->
            <button class="cell bg-slate-800 hover:bg-slate-700/80 active:bg-slate-700 rounded-xl flex items-center justify-center text-4xl md:text-5xl font-bold transition-all duration-150 focus:outline-none" data-index="0"></button>
            <button class="cell bg-slate-800 hover:bg-slate-700/80 active:bg-slate-700 rounded-xl flex items-center justify-center text-4xl md:text-5xl font-bold transition-all duration-150 focus:outline-none" data-index="1"></button>
            <button class="cell bg-slate-800 hover:bg-slate-700/80 active:bg-slate-700 rounded-xl flex items-center justify-center text-4xl md:text-5xl font-bold transition-all duration-150 focus:outline-none" data-index="2"></button>
            <button class="cell bg-slate-800 hover:bg-slate-700/80 active:bg-slate-700 rounded-xl flex items-center justify-center text-4xl md:text-5xl font-bold transition-all duration-150 focus:outline-none" data-index="3"></button>
            <button class="cell bg-slate-800 hover:bg-slate-700/80 active:bg-slate-700 rounded-xl flex items-center justify-center text-4xl md:text-5xl font-bold transition-all duration-150 focus:outline-none" data-index="4"></button>
            <button class="cell bg-slate-800 hover:bg-slate-700/80 active:bg-slate-700 rounded-xl flex items-center justify-center text-4xl md:text-5xl font-bold transition-all duration-150 focus:outline-none" data-index="5"></button>
            <button class="cell bg-slate-800 hover:bg-slate-700/80 active:bg-slate-700 rounded-xl flex items-center justify-center text-4xl md:text-5xl font-bold transition-all duration-150 focus:outline-none" data-index="6"></button>
            <button class="cell bg-slate-800 hover:bg-slate-700/80 active:bg-slate-700 rounded-xl flex items-center justify-center text-4xl md:text-5xl font-bold transition-all duration-150 focus:outline-none" data-index="7"></button>
            <button class="cell bg-slate-800 hover:bg-slate-700/80 active:bg-slate-700 rounded-xl flex items-center justify-center text-4xl md:text-5xl font-bold transition-all duration-150 focus:outline-none" data-index="8"></button>
        </div>

        <!-- Tombol Aksi (Awal Ulang & Reset Skor) -->
        <div class="flex gap-3 w-full">
            <button id="reset-board-btn" class="flex-1 bg-indigo-600 hover:bg-indigo-500 active:bg-indigo-700 text-white font-semibold py-3 px-4 rounded-xl transition-colors duration-200 flex items-center justify-center gap-2 shadow-lg">
                <i class="fa-solid fa-rotate-right"></i> Main Lagi
            </button>
            <button id="reset-score-btn" class="bg-slate-800 hover:bg-slate-700 active:bg-slate-900 border border-slate-700 text-slate-300 font-semibold py-3 px-4 rounded-xl transition-colors duration-200 flex items-center justify-center gap-2 shadow">
                <i class="fa-solid fa-trash-can"></i> Reset Skor
            </button>
        </div>
    </main>

    <!-- Modal Hasil Pertandingan (Tampil saat ada yang menang / seri) -->
    <div id="modal" class="fixed inset-0 bg-slate-950/80 backdrop-blur-sm flex items-center justify-center p-4 opacity-0 pointer-events-none transition-opacity duration-300 z-50">
        <div class="bg-slate-800 border border-slate-700 w-full max-w-sm rounded-2xl p-6 text-center shadow-2xl transform scale-95 transition-transform duration-300" id="modal-content">
            <div id="modal-icon" class="text-6xl mb-3">🏆</div>
            <h2 id="modal-title" class="text-2xl font-bold text-white mb-2">Pemain X Menang!</h2>
            <p id="modal-desc" class="text-slate-400 mb-6">Selamat atas kemenangannya!</p>
            <button id="modal-play-again" class="w-full bg-indigo-600 hover:bg-indigo-500 text-white font-bold py-3 px-4 rounded-xl transition duration-200 shadow-lg">
                Main Lagi
            </button>
        </div>
    </div>

    <!-- Footer -->
    <footer class="text-slate-500 text-xs text-center mb-2">
        Game Tic Tac Toe Sederhana &bull; 2 Pemain
    </footer>

    <!-- Logika Game (JavaScript) -->
    <script>
        // Inisialisasi variabel state game
        let boardState = ['', '', '', '', '', '', '', '', ''];
        let currentPlayer = 'X';
        let isGameActive = true;
        let scoreX = 0;
        let scoreO = 0;
        let scoreDraw = 0;

        // Kombinasi posisi untuk menang (baris, kolom, diagonal)
        const winningConditions = [
            [0, 1, 2], [3, 4, 5], [6, 7, 8], // Baris
            [0, 3, 6], [1, 4, 7], [2, 5, 8], // Kolom
            [0, 4, 8], [2, 4, 6]             // Diagonal
        ];

        // Elemen DOM
        const cells = document.querySelectorAll('.cell');
        const currentTurnDisplay = document.getElementById('current-turn');
        const cardX = document.getElementById('card-x');
        const cardO = document.getElementById('card-o');
        const scoreXDisplay = document.getElementById('score-x');
        const scoreODisplay = document.getElementById('score-o');
        const scoreDrawDisplay = document.getElementById('score-draw');
        const resetBoardBtn = document.getElementById('reset-board-btn');
        const resetScoreBtn = document.getElementById('reset-score-btn');
        
        // Modal Elemen
        const modal = document.getElementById('modal');
        const modalContent = document.getElementById('modal-content');
        const modalIcon = document.getElementById('modal-icon');
        const modalTitle = document.getElementById('modal-title');
        const modalDesc = document.getElementById('modal-desc');
        const modalPlayAgain = document.getElementById('modal-play-again');

        // Fungsi untuk menangani klik sel
        function handleCellClick(e) {
            const clickedCell = e.currentTarget;
            const clickedIndex = parseInt(clickedCell.getAttribute('data-index'));

            // Abaikan jika sel sudah terisi atau game berakhir
            if (boardState[clickedIndex] !== '' || !isGameActive) {
                return;
            }

            // Update state & UI
            makeMove(clickedCell, clickedIndex);

            // Cek status kemenangan atau seri
            checkResult();
        }

        // Melakukan langkah pemain
        function makeMove(cellElement, index) {
            boardState[index] = currentPlayer;

            if (currentPlayer === 'X') {
                cellElement.innerHTML = `<span class="symbol-pop text-indigo-400"><i class="fa-solid fa-xmark"></i></span>`;
            } else {
                cellElement.innerHTML = `<span class="symbol-pop text-rose-400"><i class="fa-regular fa-circle"></i></span>`;
            }
        }

        // Ganti giliran pemain
        function switchTurn() {
            currentPlayer = currentPlayer === 'X' ? 'O' : 'X';
            
            if (currentPlayer === 'X') {
                currentTurnDisplay.innerHTML = `<i class="fa-solid fa-xmark text-2xl"></i> (X)`;
                currentTurnDisplay.className = 'text-indigo-400 flex items-center gap-1';
                
                // Highlight indikator giliran di papan skor
                cardX.classList.add('bg-indigo-500/20', 'border-indigo-500');
                cardX.classList.remove('border-transparent');
                cardO.classList.remove('bg-rose-500/20', 'border-rose-500');
                cardO.classList.add('border-transparent');
            } else {
                currentTurnDisplay.innerHTML = `<i class="fa-regular fa-circle text-xl"></i> (O)`;
                currentTurnDisplay.className = 'text-rose-400 flex items-center gap-1';
                
                // Highlight indikator giliran di papan skor
                cardO.classList.add('bg-rose-500/20', 'border-rose-500');
                cardO.classList.remove('border-transparent');
                cardX.classList.remove('bg-indigo-500/20', 'border-indigo-500');
                cardX.classList.add('border-transparent');
            }
        }

        // Memeriksa pemenang atau seri
        function checkResult() {
            let roundWon = false;
            let winningCombo = [];

            for (let i = 0; i < winningConditions.length; i++) {
                const condition = winningConditions[i];
                const a = boardState[condition[0]];
                const b = boardState[condition[1]];
                const c = boardState[condition[2]];

                if (a === '' || b === '' || c === '') {
                    continue;
                }

                if (a === b && b === c) {
                    roundWon = true;
                    winningCombo = condition;
                    break;
                }
            }

            if (roundWon) {
                isGameActive = false;
                highlightWinningCells(winningCombo);
                
                if (currentPlayer === 'X') {
                    scoreX++;
                    scoreXDisplay.textContent = scoreX;
                    showModal('Pemain X Menang!', 'Selamat! Pemain X berhasil memenangkan ronde ini.', '🎉', 'indigo');
                } else {
                    scoreO++;
                    scoreODisplay.textContent = scoreO;
                    showModal('Pemain O Menang!', 'Selamat! Pemain O berhasil memenangkan ronde ini.', '🎉', 'rose');
                }
                return;
            }

            // Cek apakah seri (semua sel terisi)
            const roundDraw = !boardState.includes('');
            if (roundDraw) {
                isGameActive = false;
                scoreDraw++;
                scoreDrawDisplay.textContent = scoreDraw;
                showModal('Hasil Seri!', 'Pertandingan sengit! Tidak ada yang menang.', '🤝', 'slate');
                return;
            }

            // Jika belum ada yang menang dan belum seri, ganti giliran
            switchTurn();
        }

        // Mewarnai sel yang menyebabkan kemenangan
        function highlightWinningCells(combo) {
            combo.forEach(index => {
                const bgClass = currentPlayer === 'X' ? 'bg-indigo-500/30' : 'bg-rose-500/30';
                const borderClass = currentPlayer === 'X' ? 'border-2 border-indigo-500' : 'border-2 border-rose-500';
                cells[index].classList.add(bgClass, borderClass);
            });
        }

        // Tampilkan modal hasil
        function showModal(title, desc, icon, themeColor) {
            setTimeout(() => {
                modalIcon.textContent = icon;
                modalTitle.textContent = title;
                modalDesc.textContent = desc;
                
                modal.classList.remove('opacity-0', 'pointer-events-none');
                modalContent.classList.remove('scale-95');
                modalContent.classList.add('scale-100');
            }, 300);
        }

        // Sembunyikan modal
        function hideModal() {
            modal.classList.add('opacity-0', 'pointer-events-none');
            modalContent.classList.remove('scale-100');
            modalContent.classList.add('scale-95');
        }

        // Reset papan game untuk pertarungan baru
        function resetBoard() {
            boardState = ['', '', '', '', '', '', '', '', ''];
            isGameActive = true;
            currentPlayer = 'X';

            // Reset UI sel
            cells.forEach(cell => {
                cell.innerHTML = '';
                cell.className = 'cell bg-slate-800 hover:bg-slate-700/80 active:bg-slate-700 rounded-xl flex items-center justify-center text-4xl md:text-5xl font-bold transition-all duration-150 focus:outline-none';
            });

            // Reset indikator giliran ke Pemain X
            currentTurnDisplay.innerHTML = `<i class="fa-solid fa-xmark text-2xl"></i> (X)`;
            currentTurnDisplay.className = 'text-indigo-400 flex items-center gap-1';
            
            cardX.classList.add('bg-indigo-500/20', 'border-indigo-500');
            cardX.classList.remove('border-transparent');
            cardO.classList.remove('bg-rose-500/20', 'border-rose-500');
            cardO.classList.add('border-transparent');

            hideModal();
        }

        // Reset papan sekaligus skor pertandingan
        function resetScore() {
            scoreX = 0;
            scoreO = 0;
            scoreDraw = 0;
            scoreXDisplay.textContent = '0';
            scoreODisplay.textContent = '0';
            scoreDrawDisplay.textContent = '0';
            resetBoard();
        }

        // Event Listeners
        cells.forEach(cell => cell.addEventListener('click', handleCellClick));
        resetBoardBtn.addEventListener('click', resetBoard);
        resetScoreBtn.addEventListener('click', resetScore);
        modalPlayAgain.addEventListener('click', resetBoard);
    </script>
</body>
</html>
```