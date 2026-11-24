# LHS 1140 b y c: masa, radio y zona habitable

Trabajo de Ciencias Planetarias (Universidad de Antioquia, 2026). Se caracterizan los dos planetas confirmados de LHS 1140 (TOI-256) con velocidades radiales de HARPS y fotometría de TESS, y se calcula la zona habitable de estrellas de baja masa.

- **Problema 1:** curva de velocidad radial y curva de luz de LHS 1140 b y c, masa y radio de cada planeta, y comparación con la literatura.
- **Problema 2:** zona habitable de estrellas de $0.1$ a $1.2\,M_\odot$ con un modelo de evolución estelar y Kopparapu et al. (2013).

## Contenido

| Notebook | Qué hace |
|---|---|
| `1_HARPS_LHS1140_descarga.ipynb` | Descarga los espectros de HARPS del archivo de ESO, saca el `e2ds_A` de cada `.tar` y deja la estructura que pide LBL. Descarta los espectros con S/N bajo. |
| `2_LBL_HARPS_LHS1140.ipynb` | Corre LBL (line-by-line) sobre los espectros y produce la tabla de velocidades radiales. |
| `3_Analisis_RV_LHS1140.ipynb` | Promedio por noche, periodograma GLS, ajuste de b y c, indicadores de actividad, descorrelación, prueba de cromaticidad y curvas de RV plegadas. |
| `4_LHS1140_transitos_MCMC.ipynb` | Curva de luz de TESS, BLS, ajuste de tránsitos con PyTransit y emcee, radios, masas, semiejes, prueba de excentricidad y figura del sistema. |
| `Problema2_zona_habitable.ipynb` | Zona habitable para masas de $0.1$ a $1.2\,M_\odot$ y edades de $1$ a $4.6$ Gyr. Es independiente de los otros cuatro. |

## Método

**Masa mínima** a partir de la semiamplitud de la velocidad radial $K$ (órbita circular), con $M_\star = 0.1844\,M_\odot$:

$$M_p \sin i = K \left(\frac{P}{2\pi G}\right)^{1/3} M_\star^{2/3}$$

La inclinación sale del parámetro de impacto $b$ del ajuste de tránsitos, $\cos i = b\,R_\star / a$, y el semieje de la tercera ley de Kepler:

$$a^3 = \frac{G\,(M_\star + M_p)}{4\pi^2}\,P^2$$

**Radio** a partir de la profundidad del tránsito, con $k = R_p/R_\star$ y $R_\star = 0.2159\,R_\odot$:

$$R_p = k\,R_\star$$

**Zona habitable** (Kopparapu et al. 2013): flujo estelar efectivo en cada borde, con $T_\star = T_\mathrm{eff} - 5780\,\mathrm{K}$ y los coeficientes $S_0, A, B, C, D$ de su Tabla 3, y distancia al borde a partir de la luminosidad $L_\star$:

$$S_\mathrm{eff} = S_0 + A\,T_\star + B\,T_\star^2 + C\,T_\star^3 + D\,T_\star^4, \qquad d_\mathrm{HZ} = \sqrt{\frac{L_\star/L_\odot}{S_\mathrm{eff}}}\ \text{ua}$$

## Orden de ejecución

1. `1_HARPS_LHS1140_descarga` (guarda los datos en Google Drive).
2. `2_LBL_HARPS_LHS1140` (lee de Drive y deja `RV_LHS1140.csv`).
3. `3_Analisis_RV_LHS1140` (lee `RV_LHS1140.csv`).
4. `4_LHS1140_transitos_MCMC` (usa los $K$ del análisis de RV para las masas).

`Problema2_zona_habitable` se puede correr solo.

## Requisitos

- Google Colab. Los notebooks 1 a 3 montan Google Drive.
- Notebook 2: runtime de **Python 3.12** (LBL 0.67.008 declara `>=3.12,<3.13`). LBL no está en PyPI; el notebook lo instala desde GitHub con el commit fijado. La corrida completa tarda entre 2.5 y 3 h.
- Notebook 4: internet para bajar TESS desde MAST. Los MCMC tardan unos minutos por planeta.
- Los notebooks se ejecutan de arriba abajo. Cada uno instala sus dependencias en la primera celda.

## Datos

Los datos **no están en el repositorio** porque pesan demasiado (~2 GB de `.tar` y los modelos de LBL, ~360 MB). Se regeneran con los notebooks:

- **Velocidades radiales:** espectros de HARPS del [ESO Science Archive Facility](https://archive.eso.org) (296 espectros, 293 con S/N del orden 47 $\geq 5$). Se extraen con LBL (Artigau et al. 2022).
- **Fotometría:** curvas de luz de TESS (SPOC, 120 s, PDCSAP) de los sectores 3, 30 y 107, del archivo MAST.
- Los notebooks esperan la carpeta `LHS1140_HARPS` en Google Drive. Si usas otra ruta, cambia la variable `DRIVE`.

## Resultados principales

| | LHS 1140 b | LHS 1140 c |
|---|---|---|
| $K$ ($\mathrm{m\,s^{-1}}$) | $4.78 \pm 0.45$ | $2.47 \pm 0.47$ |
| Masa ($M_\oplus$) | $7.02^{+0.67}_{-0.66}$ | $1.96 \pm 0.37$ |
| Radio ($R_\oplus$) | $1.78$ | $1.25$ |
| Semieje mayor (ua) | $0.0946$ | $0.0270$ |
| Tránsitos de TESS | 3 completos | 13 completos y 4 parciales |

- La masa de b sale $2.0\sigma$ por encima de la literatura ($5.60 \pm 0.19\,M_\oplus$); la de c coincide ($1.91 \pm 0.06\,M_\oplus$). Los residuos de la RV muestran un exceso de señal a $134.3$ d, cerca de la rotación de la estrella ($131$ d).
- Con la excentricidad libre, TESS solo da límites al 95 % de $e < 0.45$ (b) y $e < 0.44$ (c).
- Con los $K$ de la literatura ($3.80$ y $2.42\,\mathrm{m\,s^{-1}}$) las masas salen unos $5.6$ y $1.9\,M_\oplus$.

## Limitaciones

- LBL se corrió con un solo blaze para todas las épocas.
- d2v, dW y fwhm tienen correlación 1.0 en la tabla de LBL, así que cuentan como un solo indicador de actividad.
- Cada planeta se ajusta por separado, con limb darkening fijo y órbita circular.
- El ajuste con excentricidad libre no llega a 50 veces el tiempo de autocorrelación ($42$ y $46$); sus resultados son orientativos.
- Más detalle en las conclusiones del notebook 4 y en la introducción del notebook 3.

## Créditos

- Las secciones 0 a 8 del notebook 1 vienen del notebook de Juan David; el resto del notebook 1 es de Mateo.
- Notebook de tránsitos basado en `02_transit_photometry.ipynb` (WASP-21 b).

## Referencias

- Cadieux, C. et al. (2024), *New Mass and Radius Constraints on the LHS 1140 Planets: LHS 1140 b Is Either a Temperate Mini-Neptune or a Water World*, ApJL, 960, L3, arXiv:2310.15490.
- Artigau, É. et al. (2022), *Line-by-Line Velocity Measurements: an Outlier-resistant Method for Precision Velocimetry*, AJ, 164, 84.
- Mayor, M. et al. (2003), *Setting New Standards with HARPS*, The Messenger, 114, 20.
- Zechmeister, M. & Kürster, M. (2009), *The generalised Lomb-Scargle periodogram*, A&A, 496, 577.
- Ricker, G. R. et al. (2015), *Transiting Exoplanet Survey Satellite (TESS)*, JATIS, 1, 014003.
- Parviainen, H. (2015), *PyTransit*, MNRAS, 450, 3233.
- Foreman-Mackey, D. et al. (2013), *emcee: The MCMC Hammer*, PASP, 125, 306.
- Kipping, D. M. (2013), *Efficient, uninformative sampling of limb darkening coefficients for two-parameter laws*, MNRAS, 435, 2152.
- Kopparapu, R. K. et al. (2013), *Habitable Zones around Main-sequence Stars: New Estimates*, ApJ, 765, 131.
- Baraffe, I. et al. (2015), *New evolutionary models for pre-main sequence and main sequence low-mass stars down to the hydrogen-burning limit*, A&A, 577, A42.
- Dittmann, J. A. et al. (2017), Nature, 544, 333; Ment, K. et al. (2019), AJ, 157, 32; Lillo-Box, J. et al. (2020), A&A, 642, A121.

## Licencia

Por definir.
