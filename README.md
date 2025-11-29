<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Игра "Жизнь" - Расширенная версия</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            flex-direction: column;
            align-items: center;
            background-color: #f0f0f0;
            margin: 0;
            padding: 20px;
        }
        
        h1 {
            color: #333;
            margin-bottom: 10px;
        }
        
        .description {
            color: #666;
            text-align: center;
            margin-bottom: 20px;
            max-width: 600px;
        }
        
        .game-container {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 15px;
        }
        
        canvas {
            border: 2px solid #333;
            background-color: white;
            cursor: pointer;
        }
        
        .controls {
            display: flex;
            gap: 10px;
            flex-wrap: wrap;
            justify-content: center;
        }
        
        button {
            padding: 8px 15px;
            font-size: 16px;
            cursor: pointer;
            border: none;
            border-radius: 4px;
            transition: background-color 0.3s, transform 0.1s;
        }
        
        button:hover {
            transform: translateY(-2px);
        }
        
        button:active {
            transform: translateY(1px);
        }
        
        #clearBtn {
            background-color: #f44336;
            color: white;
        }
        
        #clearBtn:hover {
            background-color: #d32f2f;
        }
        
        #drawModeBtn {
            background-color: #2196F3;
            color: white;
        }
        
        #drawModeBtn:hover {
            background-color: #0b7dda;
        }
        
        #autoModeBtn {
            background-color: #4CAF50;
            color: white;
        }
        
        #autoModeBtn:hover {
            background-color: #45a049;
        }
        
        #autoModeBtn.running {
            background-color: #FF9800;
        }
        
        #autoModeBtn.running:hover {
            background-color: #F57C00;
        }
        
        #stepBtn {
            background-color: #9C27B0;
            color: white;
        }
        
        #stepBtn:hover {
            background-color: #7B1FA2;
        }
        
        #fillAllBtn {
            background-color: #607D8B;
            color: white;
        }
        
        #fillAllBtn:hover {
            background-color: #455A64;
        }
        
        #randomFillBtn {
            background-color: #795548;
            color: white;
        }
        
        #randomFillBtn:hover {
            background-color: #5D4037;
        }
        
        .color-selector {
            display: flex;
            gap: 5px;
            margin-top: 10px;
            flex-wrap: wrap;
            justify-content: center;
        }
        
        .color-option {
            width: 25px;
            height: 25px;
            border: 2px solid #333;
            border-radius: 4px;
            cursor: pointer;
        }
        
        .color-option.selected {
            border-color: #FFD700;
            box-shadow: 0 0 5px #FFD700;
        }
        
        .info {
            margin-top: 15px;
            color: #666;
            text-align: center;
            max-width: 600px;
        }
        
        .legend {
            display: flex;
            flex-wrap: wrap;
            gap: 15px;
            margin-top: 10px;
            justify-content: center;
            max-width: 800px;
        }
        
        .legend-item {
            display: flex;
            align-items: center;
            gap: 5px;
            font-size: 14px;
        }
        
        .legend-color {
            width: 15px;
            height: 15px;
            border: 1px solid #333;
        }
        
        .rules {
            margin-top: 15px;
            background-color: #fff;
            padding: 15px;
            border-radius: 5px;
            max-width: 800px;
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
        }
    </style>
</head>
<body>
    <h1>Игра "Жизнь" - Расширенная версия</h1>
    
    <div class="description">
        <p>Теперь с новыми функциями: шаг вручную, закраска всей доски и случайное заполнение!</p>
    </div>
    
    <div class="game-container">
        <canvas id="gameCanvas" width="500" height="500"></canvas>
        
        <div class="color-selector">
            <div class="color-option selected" style="background-color: black;" data-color="1" title="Обычная клетка"></div>
            <div class="color-option" style="background-color: red;" data-color="2" title="Распространяющаяся клетка"></div>
            <div class="color-option" style="background-color: blue;" data-color="3" title="Стабильная клетка"></div>
            <div class="color-option" style="background-color: green;" data-color="4" title="Клетка-создатель"></div>
            <div class="color-option" style="background-color: purple;" data-color="5" title="Клетка-убийца"></div>
            <div class="color-option" style="background-color: orange;" data-color="6" title="Клетка-переключатель"></div>
        </div>
        
        <div class="controls">
            <button id="clearBtn">Очистить доску</button>
            <button id="drawModeBtn">Режим рисования: ВКЛ</button>
            <button id="autoModeBtn">Авторежим: ВЫКЛ</button>
            <button id="stepBtn">Один шаг</button>
            <button id="fillAllBtn">Закрасить всё</button>
            <button id="randomFillBtn">Случайное заполнение</button>
        </div>
    </div>
    
    <div class="legend">
        <div class="legend-item">
            <div class="legend-color" style="background-color: black;"></div>
            <span>Обычная - закрашивает 2 случайных соседа</span>
        </div>
        <div class="legend-item">
            <div class="legend-color" style="background-color: red;"></div>
            <span>Распространяющаяся - закрашивает всех соседей красным</span>
        </div>
        <div class="legend-item">
            <div class="legend-color" style="background-color: blue;"></div>
            <span>Стабильная - не меняется, но активирует соседей</span>
        </div>
        <div class="legend-item">
            <div class="legend-color" style="background-color: green;"></div>
            <span>Создатель - создаёт клетки по диагоналям</span>
        </div>
        <div class="legend-item">
            <div class="legend-color" style="background-color: purple;"></div>
            <span>Убийца - очищает соседей и остаётся на месте</span>
        </div>
        <div class="legend-item">
            <div class="legend-color" style="background-color: orange;"></div>
            <span>Переключатель - меняет цвет соседей</span>
        </div>
    </div>
    
    <div class="rules">
        <h3>Правила игры:</h3>
        <ul>
            <li><strong>Чёрные клетки:</strong> При активации закрашивают двух случайных соседей и становятся белыми</li>
            <li><strong>Красные клетки:</strong> При активации закрашивают всех соседей в красный цвет и становятся белыми</li>
            <li><strong>Синие клетки:</strong> Не меняются при активации, но активируют всех соседей</li>
            <li><strong>Зелёные клетки:</strong> Создают клетки по диагоналям и становятся белыми</li>
            <li><strong>Фиолетовые клетки:</strong> Очищают всех соседей и остаются на месте</li>
            <li><strong>Оранжевые клетки:</strong> Меняют цвет всех соседей на случайный и становятся белыми</li>
            <li>В режиме рисования можно добавлять клетки на поле (можно зажимать мышь для рисования)</li>
            <li>В авторежиме игра автоматически выполняет шаги каждую секунду</li>
            <li>Кнопка "Один шаг" выполняет один шаг игры без включения авторежима</li>
            <li>Кнопка "Закрасить всё" заполняет всю доску выбранным цветом</li>
            <li>Кнопка "Случайное заполнение" заполняет доску случайными цветами</li>
        </ul>
    </div>

    <script>
        // Получаем элементы DOM
        const canvas = document.getElementById('gameCanvas');
        const ctx = canvas.getContext('2d');
        const clearBtn = document.getElementById('clearBtn');
        const drawModeBtn = document.getElementById('drawModeBtn');
        const autoModeBtn = document.getElementById('autoModeBtn');
        const stepBtn = document.getElementById('stepBtn');
        const fillAllBtn = document.getElementById('fillAllBtn');
        const randomFillBtn = document.getElementById('randomFillBtn');
        const colorOptions = document.querySelectorAll('.color-option');
        
        // Настройки игры
        const cellSize = 20; // Размер клетки в пикселях
        const rows = canvas.height / cellSize;
        const cols = canvas.width / cellSize;
        const autoInterval = 1000; // Интервал авторежима в миллисекундах
        
        // Состояние игры
        let cells = []; // Массив состояния клеток (0 - белая, 1 - чёрная, 2 - красная, 3 - синяя, 4 - зелёная, 5 - фиолетовая, 6 - оранжевая)
        let drawMode = true; // Режим рисования
        let autoMode = false; // Авторежим
        let autoIntervalId = null; // ID интервала авторежима
        let selectedColor = 1; // Выбранный цвет
        let isMouseDown = false; // Флаг для отслеживания зажатия мыши
        
        // Инициализация игрового поля
        function initGame() {
            cells = [];
            for (let y = 0; y < rows; y++) {
                cells[y] = [];
                for (let x = 0; x < cols; x++) {
                    cells[y][x] = 0; // Все клетки белые
                }
            }
            drawBoard();
        }
        
        // Отрисовка игрового поля
        function drawBoard() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            
            // Рисуем сетку
            ctx.strokeStyle = '#ddd';
            ctx.lineWidth = 1;
            
            for (let x = 0; x <= canvas.width; x += cellSize) {
                ctx.beginPath();
                ctx.moveTo(x, 0);
                ctx.lineTo(x, canvas.height);
                ctx.stroke();
            }
            
            for (let y = 0; y <= canvas.height; y += cellSize) {
                ctx.beginPath();
                ctx.moveTo(0, y);
                ctx.lineTo(canvas.width, y);
                ctx.stroke();
            }
            
            // Рисуем клетки
            for (let y = 0; y < rows; y++) {
                for (let x = 0; x < cols; x++) {
                    if (cells[y][x] === 1) {
                        // Чёрная клетка
                        ctx.fillStyle = 'black';
                        ctx.fillRect(x * cellSize, y * cellSize, cellSize, cellSize);
                    } else if (cells[y][x] === 2) {
                        // Красная клетка
                        ctx.fillStyle = 'red';
                        ctx.fillRect(x * cellSize, y * cellSize, cellSize, cellSize);
                    } else if (cells[y][x] === 3) {
                        // Синяя клетка
                        ctx.fillStyle = 'blue';
                        ctx.fillRect(x * cellSize, y * cellSize, cellSize, cellSize);
                    } else if (cells[y][x] === 4) {
                        // Зелёная клетка
                        ctx.fillStyle = 'green';
                        ctx.fillRect(x * cellSize, y * cellSize, cellSize, cellSize);
                    } else if (cells[y][x] === 5) {
                        // Фиолетовая клетка
                        ctx.fillStyle = 'purple';
                        ctx.fillRect(x * cellSize, y * cellSize, cellSize, cellSize);
                    } else if (cells[y][x] === 6) {
                        // Оранжевая клетка
                        ctx.fillStyle = 'orange';
                        ctx.fillRect(x * cellSize, y * cellSize, cellSize, cellSize);
                    }
                }
            }
        }
        
        // Получение случайных соседей
        function getRandomNeighbors(x, y) {
            const neighbors = [];
            
            // Проверяем всех возможных соседей
            for (let dy = -1; dy <= 1; dy++) {
                for (let dx = -1; dx <= 1; dx++) {
                    // Пропускаем саму клетку
                    if (dx === 0 && dy === 0) continue;
                    
                    const nx = x + dx;
                    const ny = y + dy;
                    
                    // Проверяем, что сосед в пределах поля
                    if (nx >= 0 && nx < cols && ny >= 0 && ny < rows) {
                        neighbors.push({x: nx, y: ny});
                    }
                }
            }
            
            // Выбираем двух случайных соседей
            const selected = [];
            if (neighbors.length > 0) {
                // Первый случайный сосед
                const idx1 = Math.floor(Math.random() * neighbors.length);
                selected.push(neighbors[idx1]);
                
                // Второй случайный сосед (если есть еще соседи)
                if (neighbors.length > 1) {
                    let idx2;
                    do {
                        idx2 = Math.floor(Math.random() * neighbors.length);
                    } while (idx2 === idx1);
                    selected.push(neighbors[idx2]);
                }
            }
            
            return selected;
        }
        
        // Получение всех соседей
        function getAllNeighbors(x, y) {
            const neighbors = [];
            
            // Проверяем всех возможных соседей
            for (let dy = -1; dy <= 1; dy++) {
                for (let dx = -1; dx <= 1; dx++) {
                    // Пропускаем саму клетку
                    if (dx === 0 && dy === 0) continue;
                    
                    const nx = x + dx;
                    const ny = y + dy;
                    
                    // Проверяем, что сосед в пределах поля
                    if (nx >= 0 && nx < cols && ny >= 0 && ny < rows) {
                        neighbors.push({x: nx, y: ny});
                    }
                }
            }
            
            return neighbors;
        }
        
        // Получение диагональных соседей
        function getDiagonalNeighbors(x, y) {
            const neighbors = [];
            
            // Проверяем только диагональные направления
            for (let dy = -1; dy <= 1; dy += 2) {
                for (let dx = -1; dx <= 1; dx += 2) {
                    const nx = x + dx;
                    const ny = y + dy;
                    
                    // Проверяем, что сосед в пределах поля
                    if (nx >= 0 && nx < cols && ny >= 0 && ny < rows) {
                        neighbors.push({x: nx, y: ny});
                    }
                }
            }
            
            return neighbors;
        }
        
        // Обработка клика по клетке
        function handleCellClick(x, y) {
            const cellX = Math.floor(x / cellSize);
            const cellY = Math.floor(y / cellSize);
            
            if (cellX >= 0 && cellX < cols && cellY >= 0 && cellY < rows) {
                if (drawMode) {
                    // В режиме рисования просто закрашиваем клетку выбранным цветом
                    cells[cellY][cellX] = selectedColor;
                } else {
                    // В игровом режиме применяем правила
                    applyCellRules(cellX, cellY);
                }
                
                drawBoard();
            }
        }
        
        // Применение правил для клетки
        function applyCellRules(x, y) {
            const cellType = cells[y][x];
            
            if (cellType === 0) return; // Пустая клетка - ничего не делаем
            
            if (cellType === 1) {
                // Чёрная клетка - закрашивает двух случайных соседей
                const neighbors = getRandomNeighbors(x, y);
                
                // Закрашиваем соседей
                neighbors.forEach(neighbor => {
                    // Не перезаписываем синие клетки
                    if (cells[neighbor.y][neighbor.x] !== 3) {
                        cells[neighbor.y][neighbor.x] = 1;
                    }
                });
                
                // Текущая клетка становится белой
                cells[y][x] = 0;
            } 
            else if (cellType === 2) {
                // Красная клетка - закрашивает всех соседей в красный
                const neighbors = getAllNeighbors(x, y);
                
                // Закрашиваем всех соседей в красный
                neighbors.forEach(neighbor => {
                    // Не перезаписываем синие клетки
                    if (cells[neighbor.y][neighbor.x] !== 3) {
                        cells[neighbor.y][neighbor.x] = 2;
                    }
                });
                
                // Текущая клетка становится белой
                cells[y][x] = 0;
            }
            else if (cellType === 3) {
                // Синяя клетка - не меняется, но активирует всех соседей
                const neighbors = getAllNeighbors(x, y);
                
                // Активируем всех соседей
                neighbors.forEach(neighbor => {
                    if (cells[neighbor.y][neighbor.x] !== 0 && cells[neighbor.y][neighbor.x] !== 3) {
                        // Активируем соседа (но не рекурсивно, чтобы избежать бесконечного цикла)
                        setTimeout(() => {
                            applyCellRules(neighbor.x, neighbor.y);
                        }, 0);
                    }
                });
                
                // Синяя клетка остается на месте
            }
            else if (cellType === 4) {
                // Зелёная клетка - создаёт клетки по диагоналям
                const neighbors = getDiagonalNeighbors(x, y);
                
                // Создаём клетки по диагоналям
                neighbors.forEach(neighbor => {
                    // Не перезаписываем синие клетки
                    if (cells[neighbor.y][neighbor.x] !== 3) {
                        cells[neighbor.y][neighbor.x] = 4;
                    }
                });
                
                // Текущая клетка становится белой
                cells[y][x] = 0;
            }
            else if (cellType === 5) {
                // Фиолетовая клетка - очищает всех соседей
                const neighbors = getAllNeighbors(x, y);
                
                // Очищаем всех соседей
                neighbors.forEach(neighbor => {
                    cells[neighbor.y][neighbor.x] = 0;
                });
                
                // Фиолетовая клетка остается на месте
            }
            else if (cellType === 6) {
                // Оранжевая клетка - меняет цвет всех соседей на случайный
                const neighbors = getAllNeighbors(x, y);
                
                // Меняем цвет всех соседей на случайный
                neighbors.forEach(neighbor => {
                    if (cells[neighbor.y][neighbor.x] !== 0 && cells[neighbor.y][neighbor.x] !== 3) {
                        const randomColor = Math.floor(Math.random() * 6) + 1;
                        cells[neighbor.y][neighbor.x] = randomColor;
                    }
                });
                
                // Текущая клетка становится белой
                cells[y][x] = 0;
            }
        }
        
        // Один шаг игры в авторежиме
        function gameStep() {
            // Создаем копию текущего состояния для расчета следующего состояния
            const newCells = [];
            for (let y = 0; y < rows; y++) {
                newCells[y] = [...cells[y]];
            }
            
            // Обрабатываем все клетки
            for (let y = 0; y < rows; y++) {
                for (let x = 0; x < cols; x++) {
                    if (cells[y][x] !== 0) {
                        // Применяем правила для каждой непустой клетки
                        applyCellRulesToNewState(x, y, newCells);
                    }
                }
            }
            
            // Обновляем состояние
            cells = newCells;
            drawBoard();
        }
        
        // Применение правил для клетки в новом состоянии (для авторежима)
        function applyCellRulesToNewState(x, y, newCells) {
            const cellType = cells[y][x];
            
            if (cellType === 1) {
                // Чёрная клетка - закрашивает двух случайных соседей
                const neighbors = getRandomNeighbors(x, y);
                
                // Закрашиваем соседей в новой матрице
                neighbors.forEach(neighbor => {
                    // Не перезаписываем синие клетки
                    if (newCells[neighbor.y][neighbor.x] !== 3) {
                        newCells[neighbor.y][neighbor.x] = 1;
                    }
                });
                
                // Текущая клетка становится белой
                newCells[y][x] = 0;
            } 
            else if (cellType === 2) {
                // Красная клетка - закрашивает всех соседей в красный
                const neighbors = getAllNeighbors(x, y);
                
                // Закрашиваем всех соседей в новой матрице
                neighbors.forEach(neighbor => {
                    // Не перезаписываем синие клетки
                    if (newCells[neighbor.y][neighbor.x] !== 3) {
                        newCells[neighbor.y][neighbor.x] = 2;
                    }
                });
                
                // Текущая клетка становится белой
                newCells[y][x] = 0;
            }
            else if (cellType === 3) {
                // Синяя клетка - не меняется, но активирует всех соседей
                // В авторежиме синие клетки просто остаются на месте
                // Соседи будут обработаны в своих собственных итерациях
            }
            else if (cellType === 4) {
                // Зелёная клетка - создаёт клетки по диагоналям
                const neighbors = getDiagonalNeighbors(x, y);
                
                // Создаём клетки по диагоналям в новой матрице
                neighbors.forEach(neighbor => {
                    // Не перезаписываем синие клетки
                    if (newCells[neighbor.y][neighbor.x] !== 3) {
                        newCells[neighbor.y][neighbor.x] = 4;
                    }
                });
                
                // Текущая клетка становится белой
                newCells[y][x] = 0;
            }
            else if (cellType === 5) {
                // Фиолетовая клетка - очищает всех соседей
                const neighbors = getAllNeighbors(x, y);
                
                // Очищаем всех соседей в новой матрице
                neighbors.forEach(neighbor => {
                    newCells[neighbor.y][neighbor.x] = 0;
                });
                
                // Фиолетовая клетка остается на месте
            }
            else if (cellType === 6) {
                // Оранжевая клетка - меняет цвет всех соседей на случайный
                const neighbors = getAllNeighbors(x, y);
                
                // Меняем цвет всех соседей на случайный в новой матрице
                neighbors.forEach(neighbor => {
                    if (newCells[neighbor.y][neighbor.x] !== 0 && newCells[neighbor.y][neighbor.x] !== 3) {
                        const randomColor = Math.floor(Math.random() * 6) + 1;
                        newCells[neighbor.y][neighbor.x] = randomColor;
                    }
                });
                
                // Текущая клетка становится белой
                newCells[y][x] = 0;
            }
        }
        
        // Запуск/остановка авторежима
        function toggleAutoMode() {
            autoMode = !autoMode;
            
            if (autoMode) {
                autoModeBtn.textContent = 'Авторежим: ВКЛ';
                autoModeBtn.classList.add('running');
                autoIntervalId = setInterval(gameStep, autoInterval);
            } else {
                autoModeBtn.textContent = 'Авторежим: ВЫКЛ';
                autoModeBtn.classList.remove('running');
                clearInterval(autoIntervalId);
            }
        }
        
        // Закрасить всю доску выбранным цветом
        function fillAllWithColor() {
            for (let y = 0; y < rows; y++) {
                for (let x = 0; x < cols; x++) {
                    cells[y][x] = selectedColor;
                }
            }
            drawBoard();
        }
        
        // Заполнить доску случайными цветами
        function fillWithRandomColors() {
            for (let y = 0; y < rows; y++) {
                for (let x = 0; x < cols; x++) {
                    // Случайный цвет от 1 до 6 (0 - белый, пропускаем)
                    cells[y][x] = Math.floor(Math.random() * 6) + 1;
                }
            }
            drawBoard();
        }
        
        // Обработчики событий мыши для рисования с зажатием
        canvas.addEventListener('mousedown', (e) => {
            isMouseDown = true;
            const rect = canvas.getBoundingClientRect();
            const x = e.clientX - rect.left;
            const y = e.clientY - rect.top;
            handleCellClick(x, y);
        });
        
        canvas.addEventListener('mousemove', (e) => {
            if (isMouseDown && drawMode) {
                const rect = canvas.getBoundingClientRect();
                const x = e.clientX - rect.left;
                const y = e.clientY - rect.top;
                handleCellClick(x, y);
            }
        });
        
        canvas.addEventListener('mouseup', () => {
            isMouseDown = false;
        });
        
        canvas.addEventListener('mouseleave', () => {
            isMouseDown = false;
        });
        
        // Обработчики событий кнопок
        clearBtn.addEventListener('click', () => {
            // Останавливаем авторежим при очистке
            if (autoMode) {
                toggleAutoMode();
            }
            initGame();
        });
        
        drawModeBtn.addEventListener('click', () => {
            drawMode = !drawMode;
            drawModeBtn.textContent = drawMode ? 
                'Режим рисования: ВКЛ' : 'Режим рисования: ВЫКЛ';
        });
        
        autoModeBtn.addEventListener('click', toggleAutoMode);
        
        stepBtn.addEventListener('click', () => {
            // Выполняем один шаг игры
            gameStep();
        });
        
        fillAllBtn.addEventListener('click', fillAllWithColor);
        
        randomFillBtn.addEventListener('click', fillWithRandomColors);
        
        // Обработчики выбора цвета
        colorOptions.forEach(option => {
            option.addEventListener('click', () => {
                // Убираем выделение со всех опций
                colorOptions.forEach(opt => opt.classList.remove('selected'));
                // Выделяем выбранную опцию
                option.classList.add('selected');
                // Устанавливаем выбранный цвет
                selectedColor = parseInt(option.getAttribute('data-color'));
            });
        });
        
        // Инициализация игры
        initGame();
    </script>
</body>
</html>
