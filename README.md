# ecommerce-dashboard
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>E-Commerce Customer Analytics Dashboard</title>
  <style>
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
    body { font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif; background: #f5f5f3; color: #1a1a19; padding: 24px; min-height: 100vh; }
    .db-wrap { max-width: 960px; margin: 0 auto; }
    .db-header { display: flex; align-items: center; justify-content: space-between; margin-bottom: 1.5rem; }
    .db-title { font-size: 20px; font-weight: 500; color: #1a1a19; }
    .db-sub { font-size: 13px; color: #6b6b67; margin-top: 4px; }
    .badge { font-size: 11px; padding: 4px 12px; border-radius: 8px; background: #eaf3de; color: #3b6d11; font-weight: 500; }
    .kpi-grid { display: grid; grid-template-columns: repeat(4, 1fr); gap: 10px; margin-bottom: 1.25rem; }
    .kpi { background: #ececea; border-radius: 8px; padding: 14px 16px; }
    .kpi-label { font-size: 12px; color: #6b6b67; margin-bottom: 6px; }
    .kpi-value { font-size: 22px; font-weight: 500; color: #1a1a19; margin-bottom: 4px; }
    .kpi-change { font-size: 12px; }
    .up { color: #3b6d11; }
    .down { color: #a32d2d; }
    .charts-row { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; margin-bottom: 12px; }
    .chart-card { background: #ffffff; border: 0.5px solid rgba(0,0,0,0.12); border-radius: 12px; padding: 16px; }
    .chart-title { font-size: 13px; font-weight: 500; color: #1a1a19; margin-bottom: 12px; }
    .legend { display: flex; flex-wrap: wrap; gap: 12px; margin-bottom: 10px; font-size: 11px; color: #6b6b67; }
    .legend span { display: flex; align-items: center; gap: 5px; }
    .legend-dot { width: 10px; height: 10px; border-radius: 2px; flex-shrink: 0; }
    .bottom-row { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; }
    .seg-table { width: 100%; border-collapse: collapse; font-size: 12px; }
    .seg-table th { color: #6b6b67; font-weight: 500; text-align: left; padding: 4px 0; border-bottom: 0.5px solid rgba(0,0,0,0.12); }
    .seg-table td { padding: 7px 0; border-bottom: 0.5px solid rgba(0,0,0,0.1); color: #1a1a19; }
    .seg-table td:last-child, .seg-table th:last-child { text-align: right; }
    .bar-bg { background: #ececea; border-radius: 4px; height: 6px; width: 100%; }
    .bar-fill { height: 6px; border-radius: 4px; }
    @media (max-width: 600px) {
      .kpi-grid { grid-template-columns: repeat(2, 1fr); }
      .charts-row, .bottom-row { grid-template-columns: 1fr; }
    }
  </style>
</head>
<body>
<div class="db-wrap">

  <div class="db-header">
    <div>
      <p class="db-title">Customer Analytics Dashboard</p>
      <p class="db-sub">Online E-Commerce &middot; Jan 2024 &ndash; Dec 2024</p>
    </div>
    <span class="badge">Live</span>
  </div>

  <div class="kpi-grid">
    <div class="kpi">
      <p class="kpi-label">Total Customers</p>
      <p class="kpi-value">48,320</p>
      <span class="kpi-change up">+14.2% vs last year</span>
    </div>
    <div class="kpi">
      <p class="kpi-label">Avg. Order Value</p>
      <p class="kpi-value">$87.40</p>
      <span class="kpi-change up">+6.8% vs last year</span>
    </div>
    <div class="kpi">
      <p class="kpi-label">Retention Rate</p>
      <p class="kpi-value">62.3%</p>
      <span class="kpi-change up">+3.1% vs last year</span>
    </div>
    <div class="kpi">
      <p class="kpi-label">Churn Rate</p>
      <p class="kpi-value">18.7%</p>
      <span class="kpi-change down">+2.4% vs last year</span>
    </div>
  </div>

  <div class="charts-row">
    <div class="chart-card">
      <p class="chart-title">New vs returning customers (monthly)</p>
      <div class="legend">
        <span><span class="legend-dot" style="background:#3266ad;"></span>New</span>
        <span><span class="legend-dot" style="background:#1d9e75;"></span>Returning</span>
      </div>
      <div style="position:relative;width:100%;height:180px;">
        <canvas id="newRetChart" role="img" aria-label="Monthly bar chart comparing new and returning customers across 2024.">New vs Returning customers monthly 2024.</canvas>
      </div>
    </div>
    <div class="chart-card">
      <p class="chart-title">Customer acquisition channels</p>
      <div class="legend">
        <span><span class="legend-dot" style="background:#3266ad;"></span>Organic 34%</span>
        <span><span class="legend-dot" style="background:#1d9e75;"></span>Paid 28%</span>
        <span><span class="legend-dot" style="background:#d85a30;"></span>Social 22%</span>
        <span><span class="legend-dot" style="background:#ba7517;"></span>Email 10%</span>
        <span><span class="legend-dot" style="background:#888780;"></span>Referral 6%</span>
      </div>
      <div style="position:relative;width:100%;height:180px;">
        <canvas id="chanChart" role="img" aria-label="Donut chart of acquisition channels.">Organic 34%, Paid 28%, Social 22%, Email 10%, Referral 6%.</canvas>
      </div>
    </div>
  </div>

  <div class="charts-row">
    <div class="chart-card">
      <p class="chart-title">Customer lifetime value trend</p>
      <div style="position:relative;width:100%;height:160px;">
        <canvas id="clvChart" role="img" aria-label="Line chart showing CLV trend across 2024.">CLV trend 2024.</canvas>
      </div>
    </div>
    <div class="chart-card">
      <p class="chart-title">Age group distribution</p>
      <div style="position:relative;width:100%;height:160px;">
        <canvas id="ageChart" role="img" aria-label="Horizontal bar chart of customer age groups.">Age groups: 18-24, 25-34, 35-44, 45-54, 55+.</canvas>
      </div>
    </div>
  </div>

  <div class="bottom-row">
    <div class="chart-card">
      <p class="chart-title">Top customer segments</p>
      <table class="seg-table">
        <thead>
          <tr><th>Segment</th><th>Customers</th><th>Avg LTV</th><th style="text-align:right">Share</th></tr>
        </thead>
        <tbody>
          <tr><td>Loyal shoppers</td><td>11,420</td><td>$312</td><td><div class="bar-bg"><div class="bar-fill" style="width:78%;background:#3266ad;"></div></div></td></tr>
          <tr><td>Bargain hunters</td><td>9,870</td><td>$94</td><td><div class="bar-bg"><div class="bar-fill" style="width:60%;background:#1d9e75;"></div></div></td></tr>
          <tr><td>One-time buyers</td><td>15,200</td><td>$61</td><td><div class="bar-bg"><div class="bar-fill" style="width:45%;background:#d85a30;"></div></div></td></tr>
          <tr><td>VIP / High-value</td><td>3,140</td><td>$680</td><td><div class="bar-bg"><div class="bar-fill" style="width:95%;background:#ba7517;"></div></div></td></tr>
          <tr><td>At-risk</td><td>8,690</td><td>$128</td><td><div class="bar-bg"><div class="bar-fill" style="width:35%;background:#888780;"></div></div></td></tr>
        </tbody>
      </table>
    </div>
    <div class="chart-card">
      <p class="chart-title">Monthly customer satisfaction (NPS)</p>
      <div style="position:relative;width:100%;height:190px;">
        <canvas id="npsChart" role="img" aria-label="Line chart of monthly NPS across 2024.">NPS trend Jan-Dec 2024.</canvas>
      </div>
    </div>
  </div>

</div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.js"></script>
<script>
const months = ['Jan','Feb','Mar','Apr','May','Jun','Jul','Aug','Sep','Oct','Nov','Dec'];

new Chart(document.getElementById('newRetChart'), {
  type: 'bar',
  data: {
    labels: months,
    datasets: [
      { label: 'New', data: [1200,1350,1100,1500,1800,2100,1950,2300,2150,1800,2400,2700], backgroundColor: '#3266ad' },
      { label: 'Returning', data: [900,980,870,1100,1350,1600,1480,1750,1620,1400,1900,2100], backgroundColor: '#1d9e75' }
    ]
  },
  options: {
    responsive: true, maintainAspectRatio: false,
    plugins: { legend: { display: false } },
    scales: {
      x: { ticks: { font: { size: 10 }, color: '#888' }, grid: { display: false } },
      y: { ticks: { font: { size: 10 }, color: '#888' }, grid: { color: 'rgba(128,128,128,0.1)' } }
    }
  }
});

new Chart(document.getElementById('chanChart'), {
  type: 'doughnut',
  data: {
    labels: ['Organic','Paid Ads','Social Media','Email','Referral'],
    datasets: [{ data: [34,28,22,10,6], backgroundColor: ['#3266ad','#1d9e75','#d85a30','#ba7517','#888780'], borderWidth: 2, borderColor: '#fff' }]
  },
  options: {
    responsive: true, maintainAspectRatio: false,
    cutout: '65%',
    plugins: { legend: { display: false } }
  }
});

new Chart(document.getElementById('clvChart'), {
  type: 'line',
  data: {
    labels: months,
    datasets: [{
      label: 'Avg CLV ($)',
      data: [210,218,215,225,232,240,238,248,255,260,268,274],
      borderColor: '#3266ad', backgroundColor: 'rgba(50,102,173,0.08)',
      pointRadius: 3, tension: 0.4, fill: true
    }]
  },
  options: {
    responsive: true, maintainAspectRatio: false,
    plugins: { legend: { display: false } },
    scales: {
      x: { ticks: { font: { size: 10 }, color: '#888' }, grid: { display: false } },
      y: { ticks: { font: { size: 10 }, color: '#888', callback: v => '$' + v }, grid: { color: 'rgba(128,128,128,0.1)' } }
    }
  }
});

new Chart(document.getElementById('ageChart'), {
  type: 'bar',
  data: {
    labels: ['18–24','25–34','35–44','45–54','55+'],
    datasets: [{ data: [18,34,26,14,8], backgroundColor: ['#3266ad','#1d9e75','#d85a30','#ba7517','#888780'] }]
  },
  options: {
    indexAxis: 'y',
    responsive: true, maintainAspectRatio: false,
    plugins: { legend: { display: false } },
    scales: {
      x: { ticks: { font: { size: 10 }, color: '#888', callback: v => v + '%' }, grid: { color: 'rgba(128,128,128,0.1)' } },
      y: { ticks: { font: { size: 10 }, color: '#888' }, grid: { display: false } }
    }
  }
});

new Chart(document.getElementById('npsChart'), {
  type: 'line',
  data: {
    labels: months,
    datasets: [{
      label: 'NPS',
      data: [42,44,41,46,50,53,51,55,57,54,59,62],
      borderColor: '#1d9e75', backgroundColor: 'rgba(29,158,117,0.08)',
      pointRadius: 3, tension: 0.4, fill: true
    }]
  },
  options: {
    responsive: true, maintainAspectRatio: false,
    plugins: { legend: { display: false } },
    scales: {
      x: { ticks: { font: { size: 10 }, color: '#888' }, grid: { display: false } },
      y: { min: 30, max: 70, ticks: { font: { size: 10 }, color: '#888' }, grid: { color: 'rgba(128,128,128,0.1)' } }
    }
  }
});
</script>
</body>
</html>
