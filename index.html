<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>전국 수질 측정 데이터 시각화 대시보드</title>
    <script src="https://cdn.jsdelivr.net/npm/@tailwindcss/browser@4"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        .loader {
            border-top-color: #3498db;
            -webkit-animation: spinner 1.5s linear infinite;
            animation: spinner 1.5s linear infinite;
        }
        @-webkit-keyframes spinner { 0% { -webkit-transform: rotate(0deg); } 100% { -webkit-transform: rotate(360deg); } }
        @keyframes spinner { 0% { transform: rotate(0deg); } 100% { transform: rotate(360deg); } }
    </style>
</head>
<body class="bg-gray-50 text-gray-800 min-h-screen flex flex-col">

    <header class="bg-blue-600 text-white shadow-md py-6 px-8">
        <h1 class="text-2xl font-bold">🌊 전국 수질 측정 자료 시각화 웹앱</h1>
        <p class="text-sm opacity-90 mt-1">지점별 측정항목의 2026년 월간 추이를 실시간으로 분석합니다.</p>
    </header>

    <main class="flex-1 p-6 max-w-7xl mx-auto w-full grid grid-cols-1 md:grid-cols-4 gap-6">
        
        <div class="bg-white p-6 rounded-xl shadow-sm border border-gray-100 flex flex-col gap-4 h-fit">
            <h2 class="text-lg font-bold text-gray-700 border-b pb-2">📊 데이터 필터</h2>
            
            <div>
                <label class="block text-sm font-semibold text-gray-600 mb-1">수질 데이터 구분</label>
                <select id="categorySelect" class="w-full p-2 border rounded-lg bg-gray-50 focus:ring-2 focus:ring-blue-400 outline-none">
                    <option value="전국수질측정자료_202604.xlsx - 하천.csv">하천 수질 자료</option>
                    <option value="전국수질측정자료_202604.xlsx - 호소.csv">호소 수질 자료</option>
                    <option value="전국수질측정자료_202604.xlsx - 산단하천.csv">산단하천 수질 자료</option>
                    <option value="전국수질측정자료_202604.xlsx - 도시관류.csv">도시관류 수질 자료</option>
                </select>
            </div>

            <div>
                <label class="block text-sm font-semibold text-gray-600 mb-1">지점명 검색/선택</label>
                <select id="stationSelect" class="w-full p-2 border rounded-lg bg-gray-50 focus:ring-2 focus:ring-blue-400 outline-none" disabled>
                    <option value="">카테고리를 로딩 중입니다...</option>
                </select>
            </div>

            <div>
                <label class="block text-sm font-semibold text-gray-600 mb-1">측정항목</label>
                <select id="itemSelect" class="w-full p-2 border rounded-lg bg-gray-50 focus:ring-2 focus:ring-blue-400 outline-none" disabled>
                    <option value="">지점을 먼저 선택하세요</option>
                </select>
            </div>
            
            <div class="text-xs text-gray-400 mt-2">
                * 공백 값이나 빈 데이터는 시각화 그래프에서 자동으로 제외됩니다.
            </div>
        </div>

        <div class="md:col-span-3 flex flex-col gap-6">
            
            <div id="errorBanner" class="hidden bg-red-50 border-l-4 border-red-500 p-4 rounded-r-xl shadow-sm">
                <div class="flex items-start">
                    <div class="flex-shrink-0 text-red-500 text-xl font-bold">⚠️</div>
                    <div class="ml-3">
                        <h3 class="text-sm font-bold text-red-800" id="errorTitle">CSV 파일을 불러오지 못했습니다.</h3>
                        <div class="mt-2 text-xs text-red-700 space-y-1" id="errorMessage"></div>
                    </div>
                </div>
            </div>

            <div id="loadingOverlay" class="hidden flex flex-col items-center justify-center p-20 bg-white rounded-xl shadow-sm border border-gray-100">
                <div class="loader ease-linear rounded-full border-4 border-t-4 border-gray-200 h-12 w-12 mb-4"></div>
                <p class="text-gray-500 font-medium">데이터를 분석하고 가공하는 중입니다...</p>
            </div>

            <div id="chartContainer" class="bg-white p-6 rounded-xl shadow-sm border border-gray-100 min-h-[400px] flex items-center justify-center">
                <canvas id="waterQualityChart" class="w-full h-full hidden"></canvas>
                <p id="emptyMessage" class="text-gray-400 font-medium text-center">왼쪽 필터에서 카테고리와 지점, 항목을 선택하면 추이 그래프가 나타납니다.</p>
            </div>
            
        </div>
    </main>

    <footer class="bg-gray-100 border-t border-gray-200 text-center py-4 text-xs text-gray-500">
        &copy; 2026 전국 수질 데이터 분석 시스템. GitHub Pages 호환 빌드.
    </footer>

    <script>
        // 전역 상태 변수
        let currentCsvData = [];
        let chartInstance = null;

        // DOM 요소 캐싱
        const categorySelect = document.getElementById('categorySelect');
        const stationSelect = document.getElementById('stationSelect');
        const itemSelect = document.getElementById('itemSelect');
        const errorBanner = document.getElementById('errorBanner');
        const errorTitle = document.getElementById('errorTitle');
        const errorMessage = document.getElementById('errorMessage');
        const loadingOverlay = document.getElementById('loadingOverlay');
        const chartContainer = document.getElementById('chartContainer');
        const emptyMessage = document.getElementById('emptyMessage');
        const ctx = document.getElementById('waterQualityChart').getContext('2d');

        // CSV 심플 파서 (쉼표 및 큰따옴표 쌍 처리 포함)
        function parseCSV(text) {
            const lines = text.split(/\r?\n/);
            if (lines.length === 0 || !lines[0]) return [];
            
            const result = [];
            const headers = splitCSVLine(lines[0]);

            for (let i = 1; i < lines.length; i++) {
                if (!lines[i].trim()) continue;
                const row = splitCSVLine(lines[i]);
                if (row.length === headers.length) {
                    const obj = {};
                    headers.forEach((header, index) => {
                        obj[header.trim()] = row[index] ? row[index].trim() : '';
                    });
                    result.push(obj);
                }
            }
            return result;
        }

        function splitCSVLine(line) {
            const result = [];
            let insideQuote = false;
            let current = '';
            for (let i = 0; i < line.length; i++) {
                let char = line[i];
                if (char === '"') {
                    insideQuote = !insideQuote;
                } else if (char === ',' && !insideQuote) {
                    result.push(current);
                    current = '';
                } else {
                    current += char;
                }
            }
            result.push(current);
            return result;
        }

        // CSV 데이터 Fetch 로드 함수
        async function loadData(fileName) {
            showLoading(true);
            hideError();
            
            try {
                // GitHub Pages 호환성 확보를 위해 상대경로 사용 및 인코딩
                const response = await fetch(encodeURIComponent(fileName));
                
                if (!response.ok) {
                    throw new Error(`HTTP_STATUS_${response.status}`);
                }

                const csvText = await response.text();
                currentCsvData = parseCSV(csvText);

                if (currentCsvData.length === 0) {
                    throw new Error('EMPTY_DATA');
                }

                initFilters();
            } catch (err) {
                handleFetchError(err.message, fileName);
            } finally {
                showLoading(false);
            }
        }

        // 필터 옵션 구성
        function initFilters() {
            // Unique 지점명 추출
            const stations = [...new Set(currentCsvData.map(item => item['지점명']))].filter(Boolean).sort();
            
            stationSelect.innerHTML = '<option value="">-- 지점명을 선택하세요 --</option>';
            stations.forEach(station => {
                const opt = document.createElement('option');
                opt.value = station;
                opt.textContent = station;
                stationSelect.appendChild(opt);
            });
            
            stationSelect.disabled = false;
            itemSelect.innerHTML = '<option value="">-- 지점을 먼저 선택하세요 --</option>';
            itemSelect.disabled = true;
            
            resetChart();
        }

        // 지점 선택 시 동작
        stationSelect.addEventListener('change', () => {
            const selectedStation = stationSelect.value;
            if (!selectedStation) {
                itemSelect.innerHTML = '<option value="">-- 지점을 먼저 선택하세요 --</option>';
                itemSelect.disabled = true;
                resetChart();
                return;
            }

            // 해당 지점의 측정항목 추출
            const filteredRows = currentCsvData.filter(row => row['지점명'] === selectedStation);
            const items = [...new Set(filteredRows.map(row => row['측정항목']))].filter(Boolean).sort();

            itemSelect.innerHTML = '<option value="">-- 측정항목을 선택하세요 --</option>';
            items.forEach(item => {
                const opt = document.createElement('option');
                opt.value = item;
                opt.textContent = item;
                itemSelect.appendChild(opt);
            });

            itemSelect.disabled = false;
            resetChart();
        });

        // 측정항목 선택 시 그래프 렌더링
        itemSelect.addEventListener('change', () => {
            const station = stationSelect.value;
            const item = itemSelect.value;

            if (!station || !item) {
                resetChart();
                return;
            }

            // 타겟 데이터 추출
            const dataRow = currentCsvData.find(row => row['지점명'] === station && row['측정항목'] === item);

            if (!dataRow) {
                resetChart();
                return;
            }

            // 1월부터 12월까지 데이터 매핑 (제공된 파일의 정확한 열 이름 헤더 매칭)
            // 일부 공백 인덱싱 이슈 대응을 위해 '2026년 10월' 등 예외 이름도 유연하게 검증합니다.
            const months = [
                '2026년1월', '2026년2월', '2026년3월', '2026년4월', 
                '2026년5월', '2026년6월', '2026년7월', '2026년8월', 
                '2026년9월', '2026년 10월', '2026년11월', '2026년12월'
            ];

            const chartLabels = ['1월', '2월', '3월', '4월', '5월', '6월', '7월', '8월', '9월', '10월', '11월', '12월'];
            const chartData = months.map(m => {
                const val = dataRow[m];
                return (val !== undefined && val !== '') ? parseFloat(val) : null;
            });

            renderChart(`${station} - ${item} 월간 변화 추이`, chartLabels, chartData);
        });

        // Chart.js 그래프 그리기
        function renderChart(label, labels, data) {
            document.getElementById('waterQualityChart').classList.remove('hidden');
            emptyMessage.classList.add('hidden');

            if (chartInstance) {
                chartInstance.destroy();
            }

            chartInstance = new Chart(ctx, {
                type: 'line',
                data: {
                    labels: labels,
                    datasets: [{
                        label: label,
                        data: data,
                        borderColor: '#2563eb',
                        backgroundColor: 'rgba(37, 99, 235, 0.1)',
                        borderWidth: 2,
                        pointBackgroundColor: '#1d4ed8',
                        pointRadius: 4,
                        spanGaps: true // 데이터가 비어있을(null) 경우 선을 끊지 않고 이어줌
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    scales: {
                        y: {
                            beginAtZero: false,
                            title: { display: true, text: '측정치' }
                        },
                        x: {
                            title: { display: true, text: '2026년 측정월' }
                        }
                    },
                    plugins: {
                        legend: { position: 'top' }
                    }
                }
            });
        }

        // 카테고리 전환 이벤트 토글
        categorySelect.addEventListener('change', (e) => {
            loadData(e.target.value);
        });

        // UI 유틸리티 함수들
        function showLoading(isLoading) {
            if (isLoading) {
                loadingOverlay.classList.remove('hidden');
                document.getElementById('waterQualityChart').classList.add('hidden');
                emptyMessage.classList.add('hidden');
            } else {
                loadingOverlay.classList.add('hidden');
            }
        }

        function resetChart() {
            if (chartInstance) chartInstance.destroy();
            document.getElementById('waterQualityChart').classList.add('hidden');
            emptyMessage.classList.remove('hidden');
        }

        function hideError() {
            errorBanner.classList.add('hidden');
        }

        // 에러 가이드라인 핸들러
        function handleFetchError(errType, fileName) {
            errorBanner.classList.remove('hidden');
            resetChart();
            
            let desc = '';
            if (errType.startsWith('HTTP_STATUS_404')) {
                desc = `
                    <strong>원인:</strong> 서버(또는 GitHub Pages) 경로에 파일이 존재하지 않습니다.<br>
                    <strong>해결 방법:</strong>
                    <ul class="list-disc pl-5 mt-1 space-y-1">
                        <li>리포지토리에 파일이 정확하게 업로드되었는지 확인하세요.</li>
                        <li>파일명이 정확히 <code class="bg-red-100 px-1 rounded">${fileName}</code> 인지 확인하세요. (<strong>공백 및 대소문자</strong> 주의)</li>
                        <li>이 <code class="bg-red-100 px-1 rounded">index.html</code> 파일과 <strong>동일한 디렉토리(루트)</strong>에 업로드해야 합니다.</li>
                    </ul>
                `;
            } else if (errType === 'EMPTY_DATA') {
                desc = `<strong>원인:</strong> 파일을 찾았으나 파싱 결과 데이터가 비어있거나 올바른 행이 없습니다.<br><strong>해결 방법:</strong> CSV 데이터 인코딩 구조와 헤더가 정상 상태인지 원본 파일을 점검하세요.`;
            } else {
                desc = `
                    <strong>원인:</strong> 로컬 환경에서의 브라우저 보안 정책(CORS) 또는 알 수 없는 네트워크 문제입니다.<br>
                    <strong>해결 방법:</strong> VS Code 등을 사용 중이시라면 <code class="bg-red-100 px-1 rounded">Live Server</code> 확장 프로그램을 사용하여 로컬 서버 환경을 열고 테스트하시거나, <strong>GitHub Pages에 푸시(Push)하여 배포된 주소</strong>로 접근하면 해결됩니다.
                `;
            }
            errorMessage.innerHTML = desc;
        }

        // 초기 앱 구동 시 첫 번째 카테고리 로드
        window.addEventListener('DOMContentLoaded', () => {
            loadData(categorySelect.value);
        });
    </script>
</body>
</html>
