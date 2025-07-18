import pandas as pd
from collections import defaultdict

# Cargar datos desde Excel
file_path = "AUSBAD Tarea 9 Cuestionario Grupo E.xlsx"
df = pd.read_excel(file_path)

# Filtrar filas con preguntas válidas (excluir encabezados)
df_preguntas = df[df['Cuestionario'] != 'Dominio']

# Diccionario para agrupar preguntas por dominio
cuestionario = defaultdict(list)

for _, row in df_preguntas.iterrows():
    dominio = row['Cuestionario']
    pregunta = row['Unnamed: 1']
    cuestionario[dominio].append(pregunta)

# Diccionario para guardar respuestas
respuestas = defaultdict(list)

print("\nResponde las siguientes preguntas usando la escala del 1 al 5:")

for dominio, preguntas in cuestionario.items():
    print(f"\n--- {dominio} ---")
    for i, pregunta in enumerate(preguntas, 1):
        while True:
            try:
                respuesta = int(input(f"{i}. {pregunta}\nTu respuesta (1-5): "))
                if 1 <= respuesta <= 5:
                    respuestas[dominio].append(respuesta)
                    break
                else:
                    print("Por favor, ingresa un número entre 1 y 5.")
            except ValueError:
                print("Entrada no válida. Intenta de nuevo.")

# Calcular promedios
print("\n\nResultados por Dominio:")
for dominio, valores in respuestas.items():
    promedio = sum(valores) / len(valores)
    interpretacion = ""
    if promedio <= 2.0:
        interpretacion = "RIESGO ALTO – Se requiere intervención inmediata."
    elif promedio <= 3.5:
        interpretacion = "RIESGO MEDIO – Se recomienda tomar acciones."
    else:
        interpretacion = "RIESGO BAJO – Controles adecuados."

    print(f"{dominio}: Promedio = {promedio:.2f} → {interpretacion}")
