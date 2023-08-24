<!DOCTYPE html>
<html>
<head>
    <title>Social Media Analyzer</title>
</head>
<body>
    <h1>Social Media Analyzer</h1>
    <form method="POST" id="searchForm">
        <label for="keyword">Enter a keyword or hashtag:</label>
        <input type="text" id="keyword" name="keyword" required>
        <button type="submit">Analyze</button>
    </form>

    <div id="resultsContainer" style="display: none;">
        <h2>Results for "<span id="keywordSpan"></span>"</h2>
        <canvas id="hashtagChart"></canvas>
    </div>

    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <script>
        document.addEventListener('DOMContentLoaded', function () {
            const searchForm = document.getElementById('searchForm');
            const resultsContainer = document.getElementById('resultsContainer');
            const keywordSpan = document.getElementById('keywordSpan');

            searchForm.addEventListener('submit', async function (event) {
                event.preventDefault();

                const keyword = document.getElementById('keyword').value;
                keywordSpan.textContent = keyword;

                const response = await fetch(`https://api.example.com/scrape?keyword=${encodeURIComponent(keyword)}`);
                const data = await response.json();

                const ctx = document.getElementById('hashtagChart').getContext('2d');
                const chart = new Chart(ctx, {
                    type: 'bar',
                    data: {
                        labels: data.labels,
                        datasets: [{
                            label: 'Hashtag Counts',
                            data: data.counts,
                            backgroundColor: 'rgba(75, 192, 192, 0.6)',
                            borderWidth: 1
                        }]
                    },
                    options: {
                        scales: {
                            y: {
                                beginAtZero: true
                            }
                        }
                    }
                });

                resultsContainer.style.display = 'block';
            });
        });
    </script>
</body>
</html>
