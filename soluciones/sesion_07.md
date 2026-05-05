# Soluciones - Sesión 07

## Ejercicio 1

Modelos:

1. Conversión sí/no: Bernoulli.
2. Conversiones en 200 visitas: Binomial si los ensayos son homogéneos e independientes.
3. Llamadas a servicio: Poisson si media≈varianza; Binomial Negativa si hay sobredispersión.
4. Canal elegido: Categórica/Multinomial.

## Ejercicio 2

La regresión balanceada suele mejorar sensibilidad/recall y PR-AUC en desbalance, pero puede empeorar log-loss y Brier si las probabilidades quedan menos calibradas.

El umbral óptimo por costo minimiza:

$$
C(\tau)=C_{FP}FP(\tau)+C_{FN}FN(\tau)
$$

Con $C_{FN}=6$, el umbral óptimo suele ser menor que 0.5 si se quiere evitar falsos negativos.

## Ejercicio 3

Focal loss:

$$
FL(p_t)=-\alpha_t(1-p_t)^\gamma\log(p_t)
$$

Puede mejorar foco en positivos difíciles, pero no garantiza mejor calibración. Por eso se compara log-loss, Brier y PR-AUC, no solo focal loss.
