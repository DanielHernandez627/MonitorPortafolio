# Prompt Maestro: Generación de Hoja de Cálculo en Google Sheets

Este documento contiene el prompt estructurado listo para usar en **Gemini en Google Sheets** (o ChatGPT / Claude) para crear o regenerar el libro de seguimiento financiero del portafolio desde cero.

> **💡 Nota:** Si prefieres no usar IA, simplemente sube a tu Google Drive el archivo preconfigurado [`Plantilla_Monitoreo_Portafolio.xlsx`](./Plantilla_Monitoreo_Portafolio.xlsx) que ya incluye todas las fórmulas y pestañas listas.

---

## Prompt para Copiar y Pegar

```text
Actúa como un Arquitecto de Datos Financieros para Google Sheets y n8n.
Necesito que crees y estructures un libro de cálculo en Google Sheets llamado "Monitoreo_Portafolio_Trii_BVC" bajo una arquitectura desacoplada y orientada a eventos (Data-Driven) con soporte bimoneda (COP y USD) para monitorear inversiones de la Bolsa de Valores de Colombia (BVC), Mercado Global Colombiano (MGC) y acciones en dólares (NYSE/NASDAQ).

El libro debe tener las siguientes 4 pestañas principales:

### PESTAÑA 1: "Portafolio_Base"
Configura exactamente estas 19 columnas en la fila 1:
- Columna A: Ticker (Texto, Centrado)
- Columna B: DIR_COTIZACION (Texto, Centrado - Símbolo exacto que n8n consulta en Yahoo Finance)
- Columna C: Nombre (Texto, Alineado a la izquierda)
- Columna D: Mercado (BVC / MGC / USD, Centrado)
- Columna E: Tipo (Accion / ETF / Fondo, Centrado)
- Columna F: Moneda (COP / USD, Centrado)
- Columna G: TRM (Tasa de cambio: 1 para activos COP; para activos USD usar la fórmula: =IFERROR(GOOGLEFINANCE("CURRENCY:USDCOP"); 4150))
- Columna H: Cantidad (Número con formato decimal #,##0.00)
- Columna I: Precio Promedio Compra (Moneda origen: COP o USD)
- Columna J: Precio Mercado Actual (Moneda origen: campo actualizado vía n8n)
- Columna K: Inversión Inicial (Moneda Origen) [Fórmula: =H2*I2]
- Columna L: Valor Mercado Actual (Moneda Origen) [Fórmula: =H2*J2]
- Columna M: Ganancia No Realizada (Moneda Origen) [Fórmula: =L2-K2]
- Columna N: Inversión Inicial (COP) [Fórmula: =IF(F2="COP"; K2; K2*G2)]
- Columna O: Valor Mercado Actual (COP) [Fórmula: =IF(F2="COP"; L2; L2*G2)]
- Columna P: Ganancia No Realizada (COP) [Fórmula: =O2-N2]
- Columna Q: Rendimiento Porcentual [Fórmula: =IFERROR(P2/N2; 0) con formato 0.00%]
- Columna R: Dividendos Recibidos (COP) [Moneda COP: $ #,##0.00]
- Columna S: Variación Día (%) [Formato Porcentaje con signo: +0.00%;-0.00% - Actualizado por n8n]

Inserta 15 posiciones de demostración:
1. NVDACO | NVDACO.CL | NVIDIA Corporation | MGC | Accion | COP | 1 | 1.0 | 422640.0 | 735020.0 | 781.15
2. MINEROS | MINEROS.CL | Mineros S.A. | BVC | Accion | COP | 1 | 2.0 | 18200.0 | 22860.0 | 339.10
3. LTMCO | LTMCO.CL | Latam Airlines | MGC | Accion | COP | 1 | 294.0 | 59.50 | 85.00 | 884.78
4. IUITCO | IUITCO.CL | iShares S&P 500 Tech | MGC | ETF | COP | 1 | 4.0 | 85965.0 | 183440.0 | 0.0
5. ISA | ISA.CL | Interconexion Electrica SA | BVC | Accion | COP | 1 | 7.0 | 22802.50 | 30020.0 | 11163.0
6. ICOLCAP | ICOLCAP.CL | iShares Colcap ETF | BVC | ETF | COP | 1 | 12.0 | 11916.37 | 25846.0 | 48030.52
7. ICHNCO | ICHNCO.CL | iShares MSCI China UCITS | MGC | ETF | COP | 1 | 6.0 | 22600.0 | 18344.0 | 0.0
8. GEB | GEB.CL | Grupo Energia Bogota | BVC | Accion | COP | 1 | 40.0 | 2836.25 | 3080.0 | 8397.91
9. ETB | ETB.CL | Empresa Telecomunicaciones Bogota | BVC | Accion | COP | 1 | 317.0 | 45.0 | 135.0 | 0.0
10. ENELAMCO | ENELAMCO.CL | Enel Americas SA | MGC | Accion | COP | 1 | 100.0 | 401.0 | 295.0 | 1719.29
11. ECOPETROL | ECOPETROL.CL | Ecopetrol S.A. | BVC | Accion | COP | 1 | 86.0 | 1995.25 | 2740.0 | 38468.99
12. CIBEST | CIBEST.CL | Bancolombia | BVC | Accion | COP | 1 | 2.0 | 35100.0 | 89600.0 | 6164.0
13. AMDVASCO | AMDVASCCO.CL | DVA Silicon Fund | MGC | Fondo/Accion | COP | 1 | 6.0 | 14300.0 | 34220.0 | 765.60
14. NU | NU | Nu Holdings Ltd. | USD | Accion | USD | =IFERROR(GOOGLEFINANCE("CURRENCY:USDCOP"); 4150) | 2.0 | 12.45 | 12.44 | 0.0
15. ACON | ACON | Aclarion, Inc. | USD | Accion | USD | =IFERROR(GOOGLEFINANCE("CURRENCY:USDCOP"); 4150) | 2.0 | 2.59 | 2.39 | 0.0

En la Fila 17, coloca la Fila de Totales consolidados:
- Columna A: "Total"
- Columna N (Inversión Total COP): =SUM(N2:N16)
- Columna O (Valor Mercado Total COP): =SUM(O2:O16)
- Columna P (Ganancia Total COP): =SUM(P2:P16)
- Columna Q (Rendimiento Consolidado): =IFERROR(P17/N17; 0)
- Columna R (Total Dividendos): =SUM(R2:R16)
- Columna S (Variación Diaria Ponderada): =IFERROR(SUMPRODUCT(O2:O16; S2:S16)/SUM(O2:O16); 0)

---

### PESTAÑA 2: "Resumen_Global"
Crea un panel de consolidación patrimonial con estas métricas:
- Valor Mercado Acciones y ETFs: =Portafolio_Base!O17
- Inversión Inicial Total: =Portafolio_Base!N17
- Ganancia No Realizada Total: =Portafolio_Base!P17
- Rendimiento Ponderado Global: =Portafolio_Base!Q17
- Total Dividendos Cobrados: =Portafolio_Base!R17
- Fondos de Inversión (Trii FICs): $ 26.364,08
- Saldo Disponible en Efectivo: $ 14.457,28
- PATRIMONIO TOTAL EN TRII: =(B1+B6+B7)

---

### PESTAÑA 3: "Historico_Snapshots"
Estructura de 7 columnas para registros horarios de evolución patrimonial:
1. Fecha y Hora (Formato: YYYY-MM-DD HH:mm:ss)
2. Inversión Inicial (COP)
3. Valor Mercado Acciones/ETFs (COP)
4. Fondos Trii (COP)
5. Saldo Efectivo (COP)
6. Patrimonio Total (COP)
7. Rendimiento %

---

### PESTAÑA 4: "Historico_Precios_Activos"
Estructura de 9 columnas para auditoría individual de cada activo:
1. Fecha y Hora
2. Ticker
3. Nombre
4. Mercado
5. Precio Actual (COP)
6. Cierre Previo (COP)
7. Variación Día (COP)
8. Variación Día (%)
9. Volumen

Aplica formato visual corporativo: cabeceras azul marino (#1A365D), texto en blanco y negrita, cuadrícula gris suave y formato condicional verde/rojo para rentabilidades.
```
