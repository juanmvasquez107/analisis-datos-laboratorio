# Análisis de datos experimentales — Laboratorio de Física

Dos análisis de datos de laboratorio hechos durante la Licenciatura en Física
(Universidad Nacional de La Plata). Cada uno parte de mediciones reales y termina
en un número con su incerteza.

El hilo común no es la física: es el trabajo de convertir una medición cruda —un
archivo de instrumento, un CSV de un sensor, una tabla de tensiones— en un
resultado que se pueda defender.

**Juan Manuel Vasquez** · Berisso, Buenos Aires · juanmvasquez107@gmail.com

---

## 1. Efecto fotoeléctrico: cuánto importa el filtro

[`efecto-fotoelectrico.ipynb`](efecto-fotoelectrico.ipynb)

Potencial de frenado contra frecuencia de la luz incidente, medido en tres
configuraciones ópticas. De la pendiente sale `h/e`, y de ahí la constante de
Planck.

El resultado es el punto del análisis:

| serie | h obtenido | desvío | R² |
|---|---|---|---|
| orden 1, filtro Pasco | 6,587×10⁻³⁴ J·s | **−0,6%** | 0,9976 |
| orden 1, filtro | 6,445×10⁻³⁴ | −2,7% | 0,9969 |
| orden 2, filtro | 6,310×10⁻³⁴ | −4,8% | 0,9613 |
| orden 1, sin filtro | 4,523×10⁻³⁴ | −31,7% | 0,9815 |
| orden 2, sin filtro | 3,230×10⁻³⁴ | −51,3% | 0,8756 |

Sin filtro el error llega al 50%: entra luz de otros órdenes de difracción. Con el
filtro adecuado se recupera el valor aceptado (6,626×10⁻³⁴ J·s) con menos de 1%.

La incerteza de cada tensión no se asume constante: se modela según la
especificación del instrumento, `σ_V = 0,5%·V + resolución`, y entra al ajuste con
`absolute_sigma=True`.

**Herramientas** · numpy · scipy `optimize.curve_fit` · matplotlib

---

## 2. Espectrometría gamma: calibración en energía y borde Compton

[`espectrometria-gamma.ipynb`](espectrometria-gamma.ipynb)

Espectros de un detector de centelleo con analizador multicanal, guardados en
formato `.spe`.

- **Parser propio del `.spe`** (`leer_spe`): ubica los delimitadores `$DATA:` y
  `$ROI:` y extrae el vector de cuentas por canal. Ninguna librería lee ese
  formato.
- Calibración canal → energía con Cs-137 (fotopico en 661,7 keV), propagando las
  incertezas de pendiente y ordenada.
- Resta del fondo ambiente, truncando en cero.
- Ajuste del **borde Compton** con una gaussiana convolucionada con un escalón
  (`scipy.special.erf`): la forma que toma un corte abrupto en energía visto por
  un detector de resolución finita.
- Comparación de la atenuación a través de plomo, cobre y madera.

Errores de Poisson en las cuentas (`σ_N = √N`), ajuste acotado con `bounds` y
`absolute_sigma=True`.

**Herramientas** · numpy · scipy (`optimize.curve_fit`, `special.erf`) · matplotlib

---

## Créditos

Las mediciones son trabajo de laboratorio en grupo. El código de análisis
publicado y los datos del efecto fotoeléctrico se tomaron junto a
**Belén Robiglio**.

## Cómo correrlos

```bash
pip install -r requirements.txt
jupyter notebook
```

Los notebooks se escribieron en Google Colab y apuntan a rutas de Drive
(`/content/...`). Para correrlos localmente hay que ajustar esas rutas a los
archivos de datos.
