<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>英超比赛结果预测与分析系统</title>
  <!-- 引入Bootstrap和Chart.js -->
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet">
  <script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.0/dist/chart.umd.min.js"></script>
  <style>
    body {
      background-color: #f8f9fa;
      font-family: 'Segoe UI', sans-serif;
    }
    
    /* 导航栏样式 */
    nav {
      background-color: #343a40;
      padding: 1em 2em;
      margin-bottom: 2rem;
    }
    
    nav a {
      color: #fff;
      margin-right: 20px;
      text-decoration: none;
      font-weight: bold;
      font-size: 16px;
      transition: color 0.3s;
    }
    
    nav a:hover {
      color: #ffc107;
      text-decoration: none;
    }
    
    nav a.active {
      color: #ffc107;
      pointer-events: none;
    }
    
    .container {
      padding: 20px;
    }
    
    .card {
      cursor: pointer;
      transition: transform 0.3s ease, box-shadow 0.3s ease;
      border: none;
      box-shadow: 0 4px 6px rgba(0,0,0,0.05);
    }
    
    .card:hover {
      transform: translateY(-5px);
      box-shadow: 0 10px 20px rgba(0,0,0,0.1);
    }
    
    .card-title {
      font-weight: bold;
    }
    
    .tab-content {
      background-color: #fff;
      border-radius: 8px;
      box-shadow: 0 4px 6px rgba(0,0,0,0.05);
      padding: 20px;
      margin-top: 20px;
    }
    
    .chart-container {
      margin-top: 20px;
    }
    
    /* 模拟后端未连接时的提示样式 */
    .backend-warning {
      background-color: #fff3cd;
      border-left: 4px solid #ffc107;
      padding: 10px;
      margin: 10px 0;
      display: none;
    }
  </style>
</head>
<body>
  <!-- 顶部导航栏 -->
  <nav class="navbar">
    <a href="#home" class="active">首页</a>
    <a href="#prediction">比分预测</a>
    <a href="#dashboard">战绩查询</a>
    <a href="#analysis">数据可视化</a>
  </nav>

  <div class="container">
    <!-- 首页内容 -->
    <section id="home" class="tab-content active">
      <h1 class="text-center mb-4">英超比赛结果预测与分析系统</h1>
      <p class="sub-title text-center text-muted mb-5">请选择功能入口</p>

      <div class="row">
        <div class="col-md-4 mb-4">
          <div class="card p-4" onclick="switchTab('prediction')">
            <svg xmlns="http://www.w3.org/2000/svg" width="64" height="64" fill="#0d6efd" class="bi bi-graph-up mx-auto d-block" viewBox="0 0 16 16">
              <path fill-rule="evenodd" d="M0 0h1v15h15v1H0V0zm14 10.293l-3.5-3.5-3.5 3.5-4-4 .707-.707L7 9.293l3.5-3.5 3.5 3.5-.707.707z"/>
            </svg>
            <div class="card-body">
              <h5 class="card-title text-center">比分预测</h5>
              <p class="card-text text-center">选择两队预测比赛结果，参考历史交锋</p>
            </div>
          </div>
        </div>

        <div class="col-md-4 mb-4">
          <div class="card p-4" onclick="switchTab('dashboard')">
            <img src="https://img.icons8.com/fluency/96/statistics.png" class="mx-auto d-block" />
            <div class="card-body">
              <h5 class="card-title text-center">战绩查询</h5>
              <p class="card-text text-center">查阅球队每场比赛记录及进球折线图</p>
            </div>
          </div>
        </div>

        <div class="col-md-4 mb-4">
          <div class="card p-4" onclick="switchTab('analysis')">
            <img src="https://img.icons8.com/color/96/combo-chart.png" class="mx-auto d-block" />
            <div class="card-body">
              <h5 class="card-title text-center">数据可视化分析</h5>
              <p class="card-text text-center">图表展示胜负、进球趋势、比赛分布等</p>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- 比分预测页面 -->
    <section id="prediction" class="tab-content">
      <h2 class="mb-4">比赛结果预测系统</h2>
      
      <div class="backend-warning" id="predictionWarning">
        <strong>提示:</strong> 未连接到后端API，当前使用模拟数据
      </div>

      <div class="form-group mb-4">
        <label for="homeTeam">主队：</label>
        <select id="homeTeam" class="form-select" style="width: 200px; display: inline-block; margin-right: 10px;">
          <option value="曼联">曼联</option>
          <option value="利物浦">利物浦</option>
          <option value="切尔西">切尔西</option>
          <option value="阿森纳">阿森纳</option>
          <option value="曼城">曼城</option>
        </select>
        
        <label for="awayTeam">客队：</label>
        <select id="awayTeam" class="form-select" style="width: 200px; display: inline-block; margin-right: 10px;">
          <option value="曼联">曼联</option>
          <option value="利物浦">利物浦</option>
          <option value="切尔西">切尔西</option>
          <option value="阿森纳">阿森纳</option>
          <option value="曼城">曼城</option>
        </select>
        
        <label for="matchDate">比赛日期：</label>
        <input type="date" id="matchDate" class="form-control" style="width: 200px; display: inline-block; margin-right: 10px;" value="2025-07-10">
        
        <button onclick="predictMatch()" class="btn btn-primary">开始预测</button>
      </div>

      <div id="result" class="mb-4">
        <h3>预测结果：</h3>
        <p id="predictionText">请选择球队和日期后点击预测</p>
      </div>

      <div class="row" id="container">
        <div id="left" class="col-md-6">
          <h3>历史交锋记录：</h3>
          <table class="table">
            <thead>
              <tr>
                <th>日期</th>
                <th>主队</th>
                <th>客队</th>
                <th>比分</th>
              </tr>
            </thead>
            <tbody id="historyTable">
              <tr>
                <td colspan="4" class="text-center">选择球队后显示历史交锋记录</td>
              </tr>
            </tbody>
          </table>
        </div>

        <div id="right" class="col-md-6">
          <h3>双方历史进球可视化：</h3>
          <div class="chart-container">
            <canvas id="scoreChart" width="400" height="200"></canvas>
          </div>
        </div>
      </div>
    </section>

    <!-- 战绩查询页面 -->
    <section id="dashboard" class="tab-content">
      <h2 class="mb-4">球队数据查询</h2>
      
      <div class="backend-warning" id="dashboardWarning">
        <strong>提示:</strong> 未连接到后端API，当前使用模拟数据
      </div>

      <div class="form-group mb-4">
        <label for="teamSelect">选择球队：</label>
        <select id="teamSelect" class="form-select" style="width: 200px; display: inline-block;">
          <option value="">--请选择--</option>
          <option value="曼联">曼联</option>
          <option value="利物浦">利物浦</option>
          <option value="切尔西">切尔西</option>
          <option value="阿森纳">阿森纳</option>
          <option value="曼城">曼城</option>
        </select>
      </div>

      <div id="tableContainer">
        <table id="matchTable" class="table">
          <thead>
            <tr>
              <th>比赛日期</th>
              <th>主队</th>
              <th>客队</th>
              <th>比分</th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td colspan="4" class="text-center">选择球队后显示比赛记录</td>
            </tr>
          </tbody>
        </table>
      </div>

      <div class="chart-container">
        <canvas id="goalsChart" height="200"></canvas>
      </div>
    </section>

    <!-- 数据可视化页面 -->
    <section id="analysis" class="tab-content">
      <h1 class="mb-4 text-center">球队可视化分析</h1>
      
      <div class="backend-warning" id="analysisWarning">
        <strong>提示:</strong> 未连接到后端API，当前使用模拟数据
      </div>

      <!-- 球队选择器 -->
      <div class="mb-4">
        <label for="teamSelectAnalysis" class="form-label">选择球队：</label>
        <select id="teamSelectAnalysis" class="form-select" style="width: 200px; display: inline-block;">
          <option value="">-- 请选择 --</option>
          <option value="曼联">曼联</option>
          <option value="利物浦">利物浦</option>
          <option value="切尔西">切尔西</option>
          <option value="阿森纳">阿森纳</option>
          <option value="曼城">曼城</option>
        </select>
      </div>

      <!-- 图表区域 -->
      <div class="row chart-container">
        <div class="col-md-6 mb-4">
          <canvas id="matchesPerYearChart" height="200"></canvas>
        </div>
        <div class="col-md-6 mb-4">
          <canvas id="avgGoalsPerMonthChart" height="200"></canvas>
        </div>
        <div class="col-md-6 mb-4">
          <canvas id="resultSummaryChart" height="200"></canvas>
        </div>
        <div class="col-md-6 mb-4">
          <canvas id="goalsOverTimeChart" height="200"></canvas>
        </div>
      </div>
    </section>
  </div>

  <script>
    // 模拟后端API响应数据
    const mockData = {
      // 比分预测相关模拟数据
      prediction: {
        pred_home: 2,
        pred_away: 1,
        result: "主队胜",
        history: [
          { date: "2025-05-15", home_team: "曼联", away_team: "利物浦", score: "1-0" },
          { date: "2025-03-20", home_team: "利物浦", away_team: "曼联", score: "2-2" },
          { date: "2024-12-10", home_team: "曼联", away_team: "利物浦", score: "3-1" },
          { date: "2024-09-05", home_team: "利物浦", away_team: "曼联", score: "1-3" }
        ],
        goals: [
          { date: "2025-05-15", home_goals: 1, away_goals: 0 },
          { date: "2025-03-20", home_goals: 2, away_goals: 2 },
          { date: "2024-12-10", home_goals: 3, away_goals: 1 },
          { date: "2024-09-05", home_goals: 1, away_goals: 3 }
        ]
      },
      
      // 战绩查询相关模拟数据
      dashboard: {
        manchester: {
          match_history: [
            { date: "2025-06-15", home: "曼联", away: "切尔西", score: "2-1" },
            { date: "2025-06-01", home: "阿森纳", away: "曼联", score: "0-0" },
            { date: "2025-05-15", home: "曼联", away: "利物浦", score: "1-0" },
            { date: "2025-05-01", home: "曼城", away: "曼联", score: "3-2" },
            { date: "2025-04-15", home: "曼联", away: "阿森纳", score: "2-0" }
          ],
          goals_over_time: [
            { date: "2025-06-15", goals: 2 },
            { date: "2025-06-01", goals: 0 },
            { date: "2025-05-15", goals: 1 },
            { date: "2025-05-01", goals: 2 },
            { date: "2025-04-15", goals: 2 }
          ]
        },
        liverpool: {
          match_history: [
            { date: "2025-06-15", home: "利物浦", away: "曼城", score: "1-1" },
            { date: "2025-06-01", home: "切尔西", away: "利物浦", score: "0-2" },
            { date: "2025-05-15", home: "曼联", away: "利物浦", score: "1-0" },
            { date: "2025-05-01", home: "利物浦", away: "阿森纳", score: "3-0" },
            { date: "2025-04-15", home: "曼城", away: "利物浦", score: "2-1" }
          ],
          goals_over_time: [
            { date: "2025-06-15", goals: 1 },
            { date: "2025-06-01", goals: 2 },
            { date: "2025-05-15", goals: 0 },
            { date: "2025-05-01", goals: 3 },
            { date: "2025-04-15", goals: 1 }
          ]
        }
      },
      
      // 数据可视化相关模拟数据
      analysis: {
        manchester: {
          matches_per_year: [
            { year: "2022", matches: 38 },
            { year: "2023", matches: 38 },
            { year: "2024", matches: 38 },
            { year: "2025", matches: 30 }
          ],
          avg_goals_per_month: [
            { month: 1, avg_goals: 1.8 },
            { month: 2, avg_goals: 2.1 },
            { month: 3, avg_goals: 1.9 },
            { month: 4, avg_goals: 2.3 },
            { month: 5, avg_goals: 2.0 },
            { month: 6, avg_goals: 1.7 }
          ],
          wins: 20,
          draws: 8,
          losses: 10,
          goals_over_time: [
            { date: "2025-06-15", team_goals: 2 },
            { date: "2025-06-01", team_goals: 0 },
            { date: "2025-05-15", team_goals: 1 },
            { date: "2025-05-01", team_goals: 2 },
            { date: "2025-04-15", team_goals: 2 },
            { date: "2025-04-01", team_goals: 3 },
            { date: "2025-03-15", team_goals: 1 }
          ]
        },
        liverpool: {
          matches_per_year: [
            { year: "2022", matches: 38 },
            { year: "2023", matches: 38 },
            { year: "2024", matches: 38 },
            { year: "2025", matches: 30 }
          ],
          avg_goals_per_month: [
            { month: 1, avg_goals: 1.9 },
            { month: 2, avg_goals: 2.0 },
            { month: 3, avg_goals: 2.2 },
            { month: 4, avg_goals: 1.8 },
            { month: 5, avg_goals: 2.1 },
            { month: 6, avg_goals: 1.6 }
          ],
          wins: 18,
          draws: 10,
          losses: 10,
          goals_over_time: [
            { date: "2025-06-15", team_goals: 1 },
            { date: "2025-06-01", team_goals: 2 },
            { date: "2025-05-15", team_goals: 0 },
            { date: "2025-05-01", team_goals: 3 },
            { date: "2025-04-15", team_goals: 1 },
            { date: "2025-04-01", team_goals: 2 },
            { date: "2025-03-15", team_goals: 2 }
          ]
        }
      }
    };

    // 标签页切换功能
    function switchTab(tabId) {
      // 隐藏所有标签页
      document.querySelectorAll('.tab-content').forEach(tab => {
        tab.classList.remove('active');
      });
      
      // 激活选中的标签页
      const tab = document.getElementById(tabId);
      if (tab) {
        tab.classList.add('active');
        
        // 更新导航栏激活状态
        document.querySelectorAll('nav a').forEach(link => {
          link.classList.remove('active');
          if (link.getAttribute('href') === `#${tabId}`) {
            link.classList.add('active');
          }
        });
      }
    }

    // 比分预测功能
    let scoreChartInstance = null;
    function predictMatch() {
      const home = document.getElementById("homeTeam").value;
      const away = document.getElementById("awayTeam").value;
      
      if (!home || !away) {
        alert("请选择主队和客队！");
        return;
      }
      
      // 显示警告提示（如果后端未连接）
      document.getElementById("predictionWarning").style.display = "block";
      
      // 获取模拟预测数据
      const predictionData = mockData.prediction;
      
      // 显示预测结果
      document.getElementById("predictionText").innerText = 
        `${home} ${predictionData.pred_home} - ${predictionData.pred_away} ${away}，预测结果：${predictionData.result}`;
      
      // 渲染历史交锋记录
      const historyTable = document.getElementById("historyTable");
      historyTable.innerHTML = "";
      predictionData.history.forEach(match => {
        const row = document.createElement("tr");
        row.innerHTML = `
          <td>${match.date}</td>
          <td>${match.home_team}</td>
          <td>${match.away_team}</td>
          <td>${match.score}</td>
        `;
        historyTable.appendChild(row);
      });
      
      // 渲染进球图表
      const ctx = document.getElementById("scoreChart").getContext("2d");
      const labels = predictionData.goals.map(g => g.date);
      const homeGoals = predictionData.goals.map(g => g.home_goals);
      const awayGoals = predictionData.goals.map(g => g.away_goals);
      
      if (scoreChartInstance) scoreChartInstance.destroy();
      
      scoreChartInstance = new Chart(ctx, {
        type: 'bar',
        data: {
          labels: labels,
          datasets: [
            {
              label: `${home} 进球`,
              data: homeGoals,
              backgroundColor: 'rgba(54, 162, 235, 0.7)',
              borderColor: 'rgba(54, 162, 235, 1)',
              borderWidth: 1
            },
            {
              label: `${away} 进球`,
              data: awayGoals,
              backgroundColor: 'rgba(255, 99, 132, 0.7)',
              borderColor: 'rgba(255, 99, 132, 1)',
              borderWidth: 1
            }
          ]
        },
        options: {
          responsive: true,
          scales: {
            y: {
              beginAtZero: true
            }
          },
          plugins: {
            legend: {
              position: 'top'
            }
          }
        }
      });
    }

    // 战绩查询功能
    let goalsChartInstance = null;
    document.getElementById("teamSelect").addEventListener("change", function() {
      const team = this.value;
      if (team) {
        // 显示警告提示（如果后端未连接）
        document.getElementById("dashboardWarning").style.display = "block";
        
        // 获取模拟数据
        const teamData = mockData.dashboard[team] || mockData.dashboard.manchester;
        
        // 渲染比赛记录表格
        const tbody = document.querySelector("#matchTable tbody");
        tbody.innerHTML = "";
        teamData.match_history.forEach(match => {
          const row = document.createElement("tr");
          row.innerHTML = `
            <td>${match.date}</td>
            <td>${match.home}</td>
            <td>${match.away}</td>
            <td>${match.score}</td>
          `;
          tbody.appendChild(row);
        });
        
        // 渲染进球折线图
        const ctx = document.getElementById("goalsChart").getContext("2d");
        if (goalsChartInstance) {
          goalsChartInstance.destroy();
        }
        goalsChartInstance = new Chart(ctx, {
          type: "line",
          data: {
            labels: teamData.goals_over_time.map(item => item.date),
            datasets: [{
              label: `${team} 每场进球数`,
              data: teamData.goals_over_time.map(item => item.goals),
              borderColor: "rgba(75, 192, 192, 1)",
              fill: false,
              tension: 0.3
            }]
          },
          options: {
            responsive: true,
            plugins: {
              legend: { display: true }
            }
          }
        });
      }
    });

    // 数据可视化功能
    let analysisCharts = {};
    document.getElementById("teamSelectAnalysis").addEventListener("change", async function() {
      const team = this.value;
      if (team) {
        // 显示警告提示（如果后端未连接）
        document.getElementById("analysisWarning").style.display = "block";
        
        // 获取模拟数据
        const teamData = mockData.analysis[team] || mockData.analysis.manchester;
        
        // 渲染所有图表
        renderMatchPerYearChart(teamData, team);
        renderAvgGoalsPerMonthChart(teamData, team);
        renderResultSummaryChart(teamData, team);
        renderGoalsOverTimeChart(teamData, team);
      }
    });
    
    function renderMatchPerYearChart(data, team) {
      const ctx = document.getElementById("matchesPerYearChart").getContext("2d");
      if (analysisCharts.matchesPerYear) analysisCharts.matchesPerYear.destroy();
      analysisCharts.matchesPerYear = new Chart(ctx, {
        type: "bar",
        data: {
          labels: data.matches_per_year.map(d => d.year),
          datasets: [{
            label: `${team} 每年比赛数`,
            data: data.matches_per_year.map(d => d.matches),
            backgroundColor: "#6c757d"
          }]
        },
        options: { responsive: true }
      });
    }
    
    function renderAvgGoalsPerMonthChart(data, team) {
      const ctx = document.getElementById("avgGoalsPerMonthChart").getContext("2d");
      if (analysisCharts.avgGoalsPerMonth) analysisCharts.avgGoalsPerMonth.destroy();
      analysisCharts.avgGoalsPerMonth = new Chart(ctx, {
        type: "line",
        data: {
          labels: data.avg_goals_per_month.map(d => `第${d.month}月`),
          datasets: [{
            label: `${team} 月均进球`,
            data: data.avg_goals_per_month.map(d => d.avg_goals),
            borderColor: "#fd7e14",
            backgroundColor: "rgba(253,126,20,0.2)",
            fill: true,
            tension: 0.3
          }]
        },
        options: { responsive: true }
      });
    }
    
    function renderResultSummaryChart(data, team) {
      const ctx = document.getElementById("resultSummaryChart").getContext("2d");
      if (analysisCharts.resultSummary) analysisCharts.resultSummary.destroy();
      analysisCharts.resultSummary = new Chart(ctx, {
        type: "bar",
        data: {
          labels: ["胜", "平", "负"],
          datasets: [{
            label: "场次",
            data: [data.wins, data.draws, data.losses],
            backgroundColor: ["#198754", "#ffc107", "#dc3545"]
          }]
        },
        options: {
          responsive: true,
          scales: { y: { beginAtZero: true } }
        }
      });
    }
    
    function renderGoalsOverTimeChart(data, team) {
      const ctx = document.getElementById("goalsOverTimeChart").getContext("2d");
      if (analysisCharts.goalsOverTime) analysisCharts.goalsOverTime.destroy();
      analysisCharts.goalsOverTime = new Chart(ctx, {
        type: "line",
        data: {
          labels: data.goals_over_time.map(d => d.date),
          datasets: [{
            label: `${team} 每场进球`,
            data: data.goals_over_time.map(d => d.team_goals),
            borderColor: "#0d6efd",
            backgroundColor: "rgba(13,110,253,0.2)",
            fill: true,
            tension: 0.3
          }]
        },
        options: { responsive: true }
      });
    }
  </script>
</body>
</html>
