<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Cheat Sheet Index</title>
<style>
  body {
    font-family: Inter, sans-serif;
    background: #f8fafc;
    color: #0f172a;
    padding: 24px;
  }
  h1 { font-size: 1.6rem; margin-bottom: 12px; }
  .btn {
    display: inline-block;
    margin: 6px 8px;
    padding: 10px 18px;
    background-color: #007bff;
    color: #ffffff;
    text-decoration: none;
    border-radius: 6px;
    font-size: 16px;
    transition: background-color 0.3s;
  }
  .btn:hover { background-color: #0056b3; }
  #buttons-container { margin-top: 20px; }
</style>
</head>
<body>

<h1>Cheat Sheet Index</h1>
<div id="buttons-container">
  Loading pages...
</div>

<script>
(async () => {
  const container = document.getElementById('buttons-container');
  container.innerHTML = '';

  // List of your pages (update this when adding new pages)
  const pages = [
    'quay.html',
    'Openshift.html',
    'helm.html'
  ];

  for (const page of pages) {
    try {
      const res = await fetch(page);
      const text = await res.text();
      const titleMatch = text.match(/<h1>(.*?)<\/h1>/i);
      const fileName = page.split('/').pop();
      const title = fileName
        .replace('.html', '')
        .replace(/[-_]/g, ' ')
        .replace(/\b\w/g, c => c.toUpperCase());

      const btn = document.createElement('a');
      btn.href = page;
      btn.className = 'btn';
      btn.textContent = title;

      container.appendChild(btn);
    } catch (err) {
      console.warn(`Failed to load ${page}:`, err);
    }
  }
})();
</script>

</body>
