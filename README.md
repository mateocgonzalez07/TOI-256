\documentclass[11pt,a4paper]{article}
\usepackage[utf8]{inputenc}
\usepackage[T1]{fontenc}
\usepackage{lmodern}

\usepackage{textcomp}
\usepackage{amsmath}
\usepackage[margin=2.5cm]{geometry}
\usepackage{booktabs}
\usepackage{tabularx}
\usepackage{enumitem}
\usepackage[colorlinks=true,urlcolor=blue,linkcolor=black]{hyperref}
\setlist{itemsep=2pt,topsep=4pt}
\setlength{\parindent}{0pt}
\setlength{\parskip}{6pt}
\newcommand{\Msun}{\ensuremath{M_\odot}}
\newcommand{\Mearth}{\ensuremath{M_\oplus}}
\newcommand{\Rearth}{\ensuremath{R_\oplus}}
\newcommand{\nb}[1]{\texttt{\detokenize{#1}}}

\title{LHS~1140 b y c: masa, radio y zona habitable}
\author{Ciencias Planetarias --- Universidad de Antioquia}
\date{2026}

\begin{document}
\maketitle

Se caracterizan los dos planetas confirmados de LHS~1140 (TOI-256) con velocidades radiales de HARPS y fotometr\'ia de TESS, y se calcula la zona habitable de estrellas de baja masa.

\begin{itemize}
  \item \textbf{Problema 1:} curva de velocidad radial y curva de luz de LHS~1140 b y c, masa y radio de cada planeta, y comparaci\'on con la literatura.
  \item \textbf{Problema 2:} zona habitable de estrellas de 0.1 a 1.2~\Msun{} con un modelo de evoluci\'on estelar y Kopparapu et al.\ (2013).
\end{itemize}

\section{Contenido}

\begin{tabularx}{\textwidth}{@{}lX@{}}
\toprule
Notebook & Qu\'e hace \\
\midrule
\nb{1_HARPS_LHS1140_descarga.ipynb} & Descarga los espectros de HARPS del archivo de ESO, saca el \nb{e2ds_A} de cada \nb{.tar} y deja la estructura que pide LBL. Descarta los espectros con S/N bajo. \\
\nb{2_LBL_HARPS_LHS1140.ipynb} & Corre LBL (\emph{line-by-line}) sobre los espectros y produce la tabla de velocidades radiales. \\
\nb{3_Analisis_RV_LHS1140.ipynb} & Promedio por noche, periodograma GLS, ajuste de b y c, indicadores de actividad, descorrelaci\'on, prueba de cromaticidad y curvas de RV plegadas. \\
\nb{4_LHS1140_transitos_MCMC.ipynb} & Curva de luz de TESS, BLS, ajuste de tr\'ansitos con PyTransit y emcee, radios, masas, semiejes, prueba de excentricidad y figura del sistema. \\
\nb{Problema2_zona_habitable.ipynb} & Zona habitable para masas de 0.1 a 1.2~\Msun{} y edades de 1 a 4.6~Gyr. Es independiente de los otros cuatro. \\
\bottomrule
\end{tabularx}

\section{Orden de ejecuci\'on}

\begin{enumerate}
  \item \nb{1_HARPS_LHS1140_descarga} (guarda los datos en Google Drive).
  \item \nb{2_LBL_HARPS_LHS1140} (lee de Drive y deja \nb{RV_LHS1140.csv}).
  \item \nb{3_Analisis_RV_LHS1140} (lee \nb{RV_LHS1140.csv}).
  \item \nb{4_LHS1140_transitos_MCMC} (usa los $K$ del an\'alisis de RV para las masas).
\end{enumerate}

\nb{Problema2_zona_habitable} se puede correr solo.

\section{Requisitos}

\begin{itemize}
  \item Google Colab. Los notebooks 1 a 3 montan Google Drive.
  \item Notebook 2: runtime de \textbf{Python 3.12} (LBL 0.67.008 declara \texttt{>=3.12,<3.13}). LBL no est\'a en PyPI; el notebook lo instala desde GitHub con el commit fijado. La corrida completa tarda entre 2.5 y 3~h.
  \item Notebook 4: internet para bajar TESS desde MAST. Los MCMC tardan unos minutos por planeta.
  \item Los notebooks se ejecutan de arriba abajo. Cada uno instala sus dependencias en la primera celda.
\end{itemize}

\section{Datos}

Los datos \textbf{no est\'an en el repositorio} porque pesan demasiado (${\sim}2$~GB de \nb{.tar} y los modelos de LBL, ${\sim}360$~MB). Se regeneran con los notebooks:

\begin{itemize}
  \item \textbf{Velocidades radiales:} espectros de HARPS del \href{https://archive.eso.org}{ESO Science Archive Facility} (296 espectros, 293 con S/N del orden 47 $\geq 5$). Se extraen con LBL (Artigau et al.\ 2022).
  \item \textbf{Fotometr\'ia:} curvas de luz de TESS (SPOC, 120~s, PDCSAP) de los sectores 3, 30 y 107, del archivo MAST.
  \item Los notebooks esperan la carpeta \nb{LHS1140_HARPS} en Google Drive. Si usas otra ruta, cambia la variable \nb{DRIVE}.
\end{itemize}

\section{Resultados principales}

\begin{table}[h]
\centering
\begin{tabular}{@{}lcc@{}}
\toprule
 & LHS~1140 b & LHS~1140 c \\
\midrule
$K$ (m/s) & $4.78 \pm 0.45$ & $2.47 \pm 0.47$ \\
Masa (\Mearth) & $7.02\,^{+0.67}_{-0.66}$ & $1.96 \pm 0.37$ \\
Radio (\Rearth) & 1.78 & 1.25 \\
Semieje mayor (ua) & 0.0946 & 0.0270 \\
Tr\'ansitos de TESS & 3 completos & 13 completos y 4 parciales \\
\bottomrule
\end{tabular}
\end{table}

\begin{itemize}
  \item La masa de b sale $2.0\sigma$ por encima de la literatura ($5.60 \pm 0.19$~\Mearth); la de c coincide ($1.91 \pm 0.06$~\Mearth). Los residuos de la RV muestran un exceso de se\~nal a 134.3~d, cerca de la rotaci\'on de la estrella (131~d).
  \item Con la excentricidad libre, TESS solo da l\'imites al 95\,\% de 0.45 (b) y 0.44 (c).
  \item Con los $K$ de la literatura (3.80 y 2.42~m/s) las masas salen unos 5.6 y 1.9~\Mearth.
\end{itemize}

\section{Limitaciones}

\begin{itemize}
  \item LBL se corri\'o con un solo blaze para todas las \'epocas.
  \item d2v, dW y fwhm tienen correlaci\'on 1.0 en la tabla de LBL, as\'i que cuentan como un solo indicador de actividad.
  \item Cada planeta se ajusta por separado, con limb darkening fijo y \'orbita circular.
  \item El ajuste con excentricidad libre no llega a 50 veces el tiempo de autocorrelaci\'on (42 y 46); sus resultados son orientativos.
  \item M\'as detalle en las conclusiones del notebook 4 y en la introducci\'on del notebook 3.
\end{itemize}

\section{Cr\'editos}

\begin{itemize}
  \item Las secciones 0 a 8 del notebook 1 vienen del notebook de Juan David; el resto del notebook 1 es de Mateo.
  \item Notebook de tr\'ansitos basado en \nb{02_transit_photometry.ipynb} (WASP-21 b).
\end{itemize}

\section{Referencias}

\begin{itemize}
  \item Cadieux, C.\ et al.\ (2024), \emph{New Mass and Radius Constraints on the LHS 1140 Planets: LHS 1140 b Is Either a Temperate Mini-Neptune or a Water World}, ApJL, 960, L3, arXiv:2310.15490.
  \item Artigau, \'E.\ et al.\ (2022), \emph{Line-by-Line Velocity Measurements: an Outlier-resistant Method for Precision Velocimetry}, AJ, 164, 84.
  \item Mayor, M.\ et al.\ (2003), \emph{Setting New Standards with HARPS}, The Messenger, 114, 20.
  \item Zechmeister, M.\ \& K\"urster, M.\ (2009), \emph{The generalised Lomb-Scargle periodogram}, A\&A, 496, 577.
  \item Ricker, G.\ R.\ et al.\ (2015), \emph{Transiting Exoplanet Survey Satellite (TESS)}, JATIS, 1, 014003.
  \item Parviainen, H.\ (2015), \emph{PyTransit}, MNRAS, 450, 3233.
  \item Foreman-Mackey, D.\ et al.\ (2013), \emph{emcee: The MCMC Hammer}, PASP, 125, 306.
  \item Kipping, D.\ M.\ (2013), \emph{Efficient, uninformative sampling of limb darkening coefficients for two-parameter laws}, MNRAS, 435, 2152.
  \item Kopparapu, R.\ K.\ et al.\ (2013), \emph{Habitable Zones around Main-sequence Stars: New Estimates}, ApJ, 765, 131.
  \item Baraffe, I.\ et al.\ (2015), \emph{New evolutionary models for pre-main sequence and main sequence low-mass stars down to the hydrogen-burning limit}, A\&A, 577, A42.
  \item Dittmann, J.\ A.\ et al.\ (2017), Nature, 544, 333; Ment, K.\ et al.\ (2019), AJ, 157, 32; Lillo-Box, J.\ et al.\ (2020), A\&A, 642, A121.
\end{itemize}

\section{Licencia}

Por definir.

\end{document}
