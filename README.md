from pathlib import Path
import zipfile

base = Path("/mnt/data/corazon_para_mi_amor")
base.mkdir(exist_ok=True)

html = """<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Para mi amor ❤️</title>
  <style>
    * { box-sizing: border-box; }
    html, body { margin: 0; width: 100%; height: 100%; overflow: hidden; }
    body {
      background: #000;
      font-family: Arial, sans-serif;
      position: relative;
    }

    #ui {
      position: absolute;
      inset: 0;
    }

    .love {
      position: absolute;
      top: 50%;
      left: 50%;
      margin: -225px 0 0 -225px;
      --i: 1;
      pointer-events: none;
    }

    .love_word {
      color: #ea80b0;
      font-size: clamp(11px, 1.5vw, 18px);
      font-weight: 600;
      letter-spacing: 1px;
      white-space: nowrap;
      text-shadow: 0 0 7px #fff, 0 0 12px #ea80b0;
      transform: translateY(-100%) rotate(-30deg);
    }

    .love_horizontal {
      animation: horizontal 10s infinite alternate ease-in-out;
      animation-delay: calc(var(--i) * -120ms);
    }

    .love_vertical {
      animation: vertical 20s infinite linear;
      animation-delay: calc(var(--i) * -120ms);
    }

    @keyframes horizontal {
      from { transform: translateX(0); }
      to   { transform: translateX(450px); }
    }

    @keyframes vertical {
      0%, 50% { transform: translateY(180px); }
      10% { transform: translateY(45px); }
      15%, 18%, 20%, 30%, 32%, 35% { transform: translateY(0); }
      22% { transform: translateY(35px); }
      24% { transform: translateY(64px); }
      25% { transform: translateY(112px); }
      26% { transform: translateY(64px); }
      28% { transform: translateY(35px); }
      40% { transform: translateY(45px); }
      71% { transform: translateY(429px); }
      75% { transform: translateY(450px); }
      79% { transform: translateY(429px); }
      100% { transform: translateY(180px); }
    }

    .message {
      position: fixed;
      bottom: 25px;
      left: 50%;
      transform: translateX(-50%);
      color: #fff;
      font-size: 15px;
      text-align: center;
      opacity: .85;
      z-index: 10;
      text-shadow: 0 0 8px #ea80b0;
    }
  </style>
</head>

<body>
  <div id="ui"></div>
  <div class="message">Para ti, mi amor ❤️</div>

  <script>
    const ui = document.getElementById("ui");
    const totalItems = 100;

    for (let i = 1; i <= totalItems; i++) {
      const love = document.createElement("div");
      love.className = "love";
      love.style.setProperty("--i", i);

      love.innerHTML = `
        <div class="love_horizontal">
          <div class="love_vertical">
            <div class="love_word">I love you</div>
          </div>
        </div>
      `;

      ui.appendChild(love);
    }
  </script>
</body>
</html>
"""

(base / "index.html").write_text(html, encoding="utf-8")

zip_path = Path("/mnt/data/corazon_para_mi_amor.zip")
with zipfile.ZipFile(zip_path, "w", zipfile.ZIP_DEFLATED) as z:
    z.write(base / "index.html", "index.html")

print(f"Listo: {zip_path}")
