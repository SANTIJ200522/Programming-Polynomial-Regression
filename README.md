#include <stdio.h>
#include <math.h>

// Conjunto de datos predefinido
double batchSizes[] = {10, 20, 30, 40, 50};    // Variable independiente (Batch Size)
double processTimes[] = {15, 25, 35, 45, 55};  // Variable dependiente (Process Time)
int n = 5; // Número de elementos en el conjunto de datos

// Variables para los parámetros de la regresión
double beta0; // Intersección
double beta1; // Pendiente
double r;     // Coeficiente de correlación
double r2;    // Coeficiente de determinación

// Función para calcular la regresión lineal
void calcularRegresion() {
    double sumX = 0, sumY = 0, sumXY = 0, sumX2 = 0;

    for (int i = 0; i < n; i++) {
        sumX += batchSizes[i];
        sumY += processTimes[i];
        sumXY += batchSizes[i] * processTimes[i];
        sumX2 += batchSizes[i] * batchSizes[i];
    }

    // Cálculo de la pendiente (beta1) y la intersección (beta0)
    beta1 = (n * sumXY - sumX * sumY) / (n * sumX2 - sumX * sumX);
    beta0 = (sumY - beta1 * sumX) / n;
}

// Función para calcular la correlación y el coeficiente de determinación
void calcularCorrelacionYCoeficienteDeterminacion() {
    double sumX = 0, sumY = 0, sumX2 = 0, sumY2 = 0, sumXY = 0;

    for (int i = 0; i < n; i++) {
        sumX += batchSizes[i];
        sumY += processTimes[i];
        sumX2 += batchSizes[i] * batchSizes[i];
        sumY2 += processTimes[i] * processTimes[i];
        sumXY += batchSizes[i] * processTimes[i];
    }

    // Cálculo del coeficiente de correlación (r)
    r = (n * sumXY - sumX * sumY) / sqrt((n * sumX2 - sumX * sumX) * (n * sumY2 - sumY * sumY));

    // Cálculo del coeficiente de determinación (R^2)
    r2 = r * r;
}

// Función para realizar una predicción de tiempo basado en un tamaño de lote
double predecir(double batchSize) {
    return beta0 + beta1 * batchSize;
}

int main() {
    // Calcular la regresión y los coeficientes
    calcularRegresion();
    calcularCorrelacionYCoeficienteDeterminacion();

    // Mostrar la ecuación de regresión
    printf("La ecuación de regresión es: Process Time = %.2f + %.2f * Batch Size\n", beta0, beta1);

    // Mostrar correlación y coeficiente de determinación
    printf("Coeficiente de correlación (r): %.2f\n", r);
    printf("Coeficiente de determinación (R^2): %.2f\n", r2);

    // Realizar cinco predicciones para valores de Batch Size
    double valoresBatchSize[] = {15, 25, 35, 45, 60}; // Valores conocidos y desconocidos
    int numValores = 5;

    for (int i = 0; i < numValores; i++) {
        double batchSize = valoresBatchSize[i];
        double tiempoPredicho = predecir(batchSize);
        printf("Para Batch Size = %.2f, el tiempo predicho es: %.2f\n", batchSize, tiempoPredicho);
    }

    return 0;
}
