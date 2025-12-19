<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Study Sheet Template</title>

  <style>
    body {
      font-family: Inter, system-ui, -apple-system, 'Segoe UI', Roboto, 'Helvetica Neue', Arial;
      line-height: 1.5;
      color: #0f172a;
      background: #f8fafc;
      padding: 24px;
    }

    .container {
      max-width: 900px;
      margin: 0 auto;
      background: #ffffff;
      padding: 28px;
      border-radius: 12px;
      box-shadow: 0 6px 20px rgba(2, 6, 23, 0.08);
    }

    h1 {
      font-size: 1.6rem;
      margin-bottom: 6px;
    }

    h2 {
      font-size: 1.1rem;
      margin-top: 18px;
    }

    h3 {
      font-size: 1rem;
      margin-top: 14px;
    }

    p {
      margin: 8px 0;
    }

    ul {
      margin: 8px 0 8px 20px;
    }

    pre {
      background: #0b1220;
      color: #e6eef8;
      padding: 12px;
      border-radius: 8px;
      overflow: auto;
    }

    code {
      font-family: SFMono-Regular, Menlo, Monaco, Consolas, 'Liberation Mono', monospace;
    }

    hr {
      margin: 24px 0;
      border: none;
      border-top: 1px solid #e5e7eb;
    }

    .badge {
      display: inline-block;
      background: #eef2ff;
      color: #3730a3;
      padding: 6px 10px;
      border-radius: 999px;
      font-weight: 600;
      font-size: 0.85rem;
    }

    .pill {
      background: #eef2ff;
      color: #3730a3;
      padding: 6px 10px;
      border-radius: 999px;
      font-weight: 600;
    }

    .key {
      background: #fffbeb;
      border-left: 4px solid #f59e0b;
      padding: 10px;
      border-radius: 6px;
    }

    .tip {
      background: #ecfeff;
      border-left: 4px solid #06b6d4;
      padding: 10px;
      border-radius: 6px;
    }

    .actions {
      display: flex;
      gap: 8px;
      flex-wrap: wrap;
      margin-top: 12px;
    }

    .btn {
      display: inline-block;
      margin: 8px 0;
      padding: 10px 18px;
      background-color: #007bff;
      color: #ffffff;
      text-decoration: none;
      border-radius: 6px;
      font-size: 16px;
      transition: background-color 0.3s;
    }

    .btn:hover {
      background-color: #0056b3;
    }

    footer {
      margin-top: 24px;
      font-size: 0.9rem;
      color: #475569;
      text-align: center;
    }
  </style>
</head>

<body>
  <div class="container">

    <!-- Navigation -->
    <div class="actions">
      <a href="#" class="btn">Home</a>
      <a href="#" class="btn">Back</a>
      <a href="#" class="btn">Section</a>
    </div>

    <!-- Page Title -->
    <h1>Page Title</h1>
    <span class="badge">Optional Badge</span>

    <!-- Intro Section -->
    <section>
      <h2>Introduction</h2>
      <p>
        Short introductory text explaining the purpose of this page.
      </p>
    </section>

    <hr>

    <!-- Content Section -->
    <section>
      <h2>Section Title</h2>
      <p>Description or explanation.</p>

      <h3>Subsection</h3>
      <ul>
        <li>Bullet point</li>
        <li>Bullet point</li>
      </ul>

      <pre><code># Code block
command --example</code></pre>
    </section>

    <hr>

    <!-- Highlight Boxes -->
    <div class="key">
      <strong>Key Point:</strong> Important concept or warning.
    </div>

    <br>

    <div class="tip">
      <strong>Tip:</strong> Best practice or recommendation.
    </div>

    <!-- Footer -->
    <footer>
      © Study Sheet Template
    </footer>

  </div>
</body>
</html>
