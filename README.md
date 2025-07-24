<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>Plast.iQ Usage Chart</title>
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <script src="https://unpkg.com/html5-qrcode" type="text/javascript"></script>
  <style>
    body {
      font-family: Arial, sans-serif;
      text-align: center;
      padding: 20px;
    }

    canvas {
      max-width: 700px;
      margin: auto;
      display: block;
    }

    #reader {
      width: 300px;
      margin: 20px auto;
    }
  </style>
</head>
<body>

  <h2>Plast.iQ Monthly Usage (2025)</h2>
  <div id="reader"></div>
  <p>Scan any QR code to increase the usage by 1 for the current month</p>
  <canvas id="usageChart"></canvas>

  <script>
    const usageData = [5, 6, 8, 7, 10, 12, 15, 18, 17, 20, 25, 30];
    const monthLabels = ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun',
                         'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'];

    const ctx = document.getElementById('usageChart').getContext('2d');
    const usageChart = new Chart(ctx, {
      type: 'bar',
      data: {
        labels: monthLabels,
        datasets: [{
          label: 'Plastic Bags Used',
          data: usageData,
          backgroundColor: '#4CAF50',
          borderRadius: 5
        }]
      },
      options: {
        scales: {
          y: {
            beginAtZero: true,
            title: {
              display: true,
              text: 'Units Used'
            }
          }
        }
      }
    });

    function addUsage() {
      const currentMonth = new Date().getMonth();
      usageChart.data.datasets[0].data[currentMonth] += 1;
      usageChart.update();
      alert(`✅ QR scanned! Added +1 to ${monthLabels[currentMonth]}`);
    }

    const qrScanner = new Html5Qrcode("reader");
    qrScanner.start(
      { facingMode: "environment" },
      { fps: 10, qrbox: 200 },
      qrCodeMessage => {
        addUsage();
        qrScanner.pause();
        setTimeout(() => qrScanner.resume(), 3000);
      },
      errorMessage => {
        // Optionally log scan errors
      }
    ).catch(err => {
      console.error("QR Scanner failed:", err);
    });
  </script>

</body>
</html>
