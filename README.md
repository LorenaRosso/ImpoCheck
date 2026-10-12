# ImpoCheck 🧮 — Calculadora Aduanera

Simulador rápido de costos estimados de importación (arancel, tasa estadística e IVA) para Argentina, distribuido como **Telegram Mini App**.

🔗 **Demo en vivo:** https://impo-check.vercel.app/
🤖 **Vía Telegram:** [@ImpoCheckBot](https://t.me/ImpoCheckBot)

---

## ¿Qué hace?

A partir del valor FOB de un producto y su categoría (Tecnología/Electrónica, Indumentaria/Textil, Varios/Insumos), calcula una estimación de:

- Arancel aduanero (según categoría)
- Tasa estadística (3%)
- IVA (21%, sobre base FOB + arancel + tasa estadística)
- Total estimado en aduana

> ⚠️ Es un simulador orientativo. Las tasas reales varían según posición arancelaria, acuerdos comerciales vigentes y normativa de AFIP/Aduana al momento de la operación — no reemplaza una liquidación oficial.

## Por qué existe

Vengo de un perfil de **Comercio Exterior y Aduana** (Técnico Superior, IFTS N°6) y de auditoría de procesos administrativos. Quería una herramienta rápida para dar una primera estimación de costos de importación sin abrir una planilla — y aprovechar que Telegram permite distribuir una mini-app sin pedirle a nadie que instale nada.

## Stack

- HTML / CSS / JavaScript vanilla (sin frameworks ni dependencias de build)
- [Telegram Mini Apps SDK](https://core.telegram.org/bots/webapps) para integrarse con el tema visual y los gestos de Telegram
- Desplegado en [Vercel](https://vercel.com)
- Bot registrado vía BotFather, con botón de menú que abre la Mini App (sin backend propio: la lógica corre 100% en el cliente)

## Cómo se construyó

El prototipo inicial de interfaz y lógica de cálculo lo generé con asistencia de IA (Gemini) para acelerar el armado del HTML/CSS. Al revisar el código antes de publicarlo encontré que el archivo generado mezclaba dos versiones del script de cálculo en un mismo bloque — una sin distinción por categoría y otra con arancel variable por categoría — lo que producía un `SyntaxError` real (token inesperado) y rompía el formulario. Lo diagnostiqué, descarté la versión duplicada y dejé una única implementación limpia y funcional.

## Cómo probarlo localmente

No requiere instalación: es un único archivo estático.

```bash
git clone https://github.com/LorenaRosso/ImpoCheck.git
cd ImpoCheck
# Abrir index.html en el navegador, o servirlo con cualquier servidor estático:
python3 -m http.server 8000
```

## Estructura

```
ImpoCheck/
├── index.html      # UI + lógica de cálculo
├── vercel.json      # Config de rewrite para Vercel
└── README.md
```
