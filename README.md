# ImpoCheck 🧮 — Calculadora Aduanera

Simulador rápido de costos estimados de importación (arancel, tasa estadística e IVA) para Argentina, distribuido como **Telegram Mini App**.

🔗 **Demo en vivo:** https://impocheck.vercel.app/
🤖 **Vía Telegram:** [@ImpoCheckBot](https://t.me/ImpoCheckBot)

---

## ¿Qué hace?

A partir del valor de la mercadería, el Incoterm® 2020 pactado, flete, seguro (si corresponde) y categoría del producto (Tecnología/Electrónica, Indumentaria/Textil, Varios/Insumos), calcula una estimación de:

- Valor en Aduana (base imponible)
- Arancel aduanero (según categoría)
- Tasa estadística (3%)
- IVA (21%, sobre Valor en Aduana + arancel + tasa estadística)
- Total estimado en aduana, en USD y convertido a ARS

### Incoterms® 2020 soportados

Los 11 términos de la ICC, agrupados según si el flete y el seguro ya vienen incluidos en el valor cotizado o hay que sumarlos aparte:

| Grupo | Incoterms | Flete | Seguro |
|---|---|---|---|
| Vendedor no asume transporte | EXW, FCA, FAS, FOB | Lo paga el comprador (sumalo en la app) | No obligatorio para el vendedor |
| Vendedor paga flete, no seguro | CFR, CPT, DAP, DPU, DDP | Ya incluido en el valor | No obligatorio para el vendedor |
| Vendedor paga flete y seguro | CIF, CIP | Ya incluido en el valor | Ya incluido en el valor |

La app habilita o deshabilita los campos de Flete y Seguro automáticamente según el Incoterm elegido, para no duplicar un costo que ya está en el valor cotizado.

> ⚠️ El campo Seguro es un monto manual: la app no calcula primas de seguro (varían por aseguradora, mercadería y ruta), solo suma lo que el usuario ingresa. Tampoco reemplaza una liquidación oficial — las tasas reales varían según posición arancelaria (NCM), acuerdos comerciales vigentes y normativa de AFIP/Aduana al momento de la operación.

### Conversión a pesos (ARS)

Al cargar, la app consulta en vivo la cotización del **dólar Banco Nación (BNA), tipo de cambio vendedor** — vía la API pública y gratuita [dolarapi.com](https://dolarapi.com) — y la usa para mostrar el total estimado también en pesos.

Esta cotización, y no el dólar "oficial" genérico, es la referencia legal para liquidar tributos de importación en Argentina (Ley 22415, art. 639). La app toma el valor vigente al momento de abrirla; la liquidación real de AFIP usa el tipo de cambio de la fecha específica de registro de ingreso aduanero, que puede ser otro día.

Si la API no responde (timeout de 6s, error de red, o formato inesperado), la app lo informa explícitamente y sigue funcionando normalmente solo en USD — no hay ningún valor de respaldo inventado.

### Validación de datos

Todos los campos numéricos (valor, flete, seguro) se validan antes de calcular: no se aceptan negativos, texto no numérico, ni valores fuera de un rango razonable. Un campo inválido muestra un error específico en vez de calcularse silenciosamente como 0 — evita que un error de tipeo pase desapercibido en el resultado. Incoterm y categoría se validan contra una lista fija de valores permitidos.

## Por qué existe

Vengo de un perfil de **Comercio Exterior y Aduana** (Técnico Superior, IFTS N°6) y de auditoría de procesos administrativos. Quería una herramienta rápida para dar una primera estimación de costos de importación sin abrir una planilla — y aprovechar que Telegram permite distribuir una mini-app sin pedirle a nadie que instale nada.

## Stack

- HTML / CSS / JavaScript vanilla (sin frameworks ni dependencias de build)
- [Telegram Mini Apps SDK](https://core.telegram.org/bots/webapps) para integrarse con el tema visual y los gestos de Telegram
- [dolarapi.com](https://dolarapi.com) para la cotización BNA en tiempo real (API pública, sin clave)
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
