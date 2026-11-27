# Cómo se actualiza `resumen.json`

Este repositorio solo guarda `resumen.json`, que lee el panel «Situación de mercado» de la interfaz Crypto Grid (abierta en local en el ordenador de Ibai). La página lo descarga de
`https://raw.githubusercontent.com/IbaiR990/mercado-resumen/main/resumen.json`.

Todo el texto va en **español de España**, claro y directo, sin jerga innecesaria. Es orientativo, nunca un consejo de inversión.

## Reglas
- Usa solo datos actuales y verificables. Nunca inventes precios, flujos, funding, cifras ni noticias.
- Si un dato no está disponible, déjalo fuera o escribe «desconocido». No adivines.
- Separa hechos de interpretación. No des las liquidaciones o niveles como objetivos garantizados.
- Plantea siempre un escenario alcista y uno bajista, con confirmación e invalidación.
- El abogado del diablo da el mejor argumento contra el sesgo principal.
- Máximo una frase citada literal por fuente; parafrasea el resto.
- Idea de fondo de Ibai: las noticias claramente bajistas abundan cerca de los mínimos y las alcistas cerca de los máximos; el mercado lo mueven sobre todo la liquidez y el crédito, y el IPC y la FOMC generan la volatilidad que él opera. Refléjalo en `sentimiento.lectura_contraria`, como aviso, no como señal.

## Pasos
1. Lee el `resumen.json` actual: conserva los `eventos` (calendario y resultados pasados).
2. Investiga con búsquedas web (Reuters, Bloomberg, CNBC, FT, WSJ, CoinDesk, The Block, Fed, BLS, BCE…): precio y tendencia de BTC/ETH y bolsas, flujos de ETF, índice de miedo y codicia, expectativas de la Fed (FedWatch), noticias clave de las últimas 24–48 h.
3. Si hoy o ayer hubo IPC, empleo (NFP), FOMC o BCE: rellena en ese evento `real`, `esperado`, `anterior`, `efecto` (`hawkish` | `dovish` | `neutral`) y `resumen` (una o dos frases, incluida la reacción del mercado). Rellena también `esperado`/`anterior` de los próximos eventos cuando haya consenso publicado.
4. Datos FRED (con WebFetch, **una petición cada vez**, si no da error de límite): `WALCL`, `WTREGEN`, `RRPONTSYD`, `DGS10`, `DTWEXBGS` (26 observaciones) y `M2SL` (13). URL: `https://api.stlouisfed.org/fred/series/observations?series_id=SERIE&api_key=CLAVE&file_type=json&limit=26&sort_order=desc`. La clave va en las instrucciones de la tarea, no en este repositorio. Si una serie falla, conserva la anterior.
5. Escribe el JSON nuevo (mismo esquema), valida que es JSON correcto, haz commit y push a `main`.

## Esquema
```json
{
  "version": 1,
  "actualizado": "2026-11-27T07:30:00+01:00",
  "sesgo": {"direccion": "alcista|bajista|neutral", "confianza": "baja|media|alta", "texto": "2-3 frases"},
  "lo_unico": "Lo único que hay que saber antes de operar hoy",
  "macro": {"texto": "párrafo", "fed": {"tipo": "", "ultimo": "", "proximo": ""}, "bce": {"tipo": "", "ultimo": "", "proximo": ""}},
  "sentimiento": {"tono": -1.0, "etiqueta": "Muy negativo", "texto": "", "lectura_contraria": ""},
  "escenarios": {
    "alcista": {"texto": "", "confirmacion": "", "invalidacion": ""},
    "bajista": {"texto": "", "confirmacion": "", "invalidacion": ""}
  },
  "abogado_diablo": "",
  "proximas_24h": ["qué vigilar", "qué cambiaría el sesgo"],
  "titulares": [{"t": "titular en español", "fuente": "", "tono": -1, "url": "", "fecha": "AAAA-MM-DD"}],
  "eventos": [{"id": "CPI-2026-12-10", "tipo": "CPI|NFP|FOMC|BCE", "nombre": "", "t": "2026-12-10T13:30:00Z",
               "real": "", "esperado": "", "anterior": "", "efecto": "hawkish|dovish|neutral", "resumen": ""}],
  "liquidez": {"fuente": "FRED", "series": {"walcl": [["AAAA-MM-DD", 0]], "tga": [], "rrp": [], "dgs10": [], "usd": [], "m2": []}, "texto": ""},
  "fuentes": [{"t": "", "url": ""}]
}
```
- `tono`: de -1 (muy negativo) a 1 (muy positivo). `titulares`: 5–8, `tono` -1, 0 o 1.
- Series en orden de fecha ascendente. Unidades FRED: WALCL y WTREGEN en millones de $, RRPONTSYD en miles de millones.
- Horas de `eventos` en UTC: IPC y NFP 08:30 Nueva York; FOMC 14:00 Nueva York; BCE 14:15 Fráncfort.
