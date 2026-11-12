### Resultados

Detalle completo de los 24 resultados (12 casos × 2 motores) en [`results/results.md`](results/results.md) y [`results/results.json`](results/results.json).

| Métrica | Tradicional | Generativo |
|---|---|---|
| Correctas (categoría y prioridad) | 10/12 (83.3 %) | 8/12 (66.7 %) |
| Categoría correcta | 11/12 (91.7 %) | 12/12 (100 %) |
| Formato válido | 12/12 | 12/12 |

| Tipo de caso | Tradicional | Generativo |
|---|---|---|
| Normales (6) | 6/6 | 4/6 |
| Ambiguos (3) | 1/3 | 3/3 |
| Adversariales (3) | 3/3 | 1/3 |

Los 4 errores del motor generativo fueron de prioridad, no de categoría. Los 2 errores del tradicional (casos 8 y 9) fueron en mensajes ambiguos.

### Conclusión técnica

El motor tradicional obtuvo 10/12 (83.3 %) clasificaciones correctas y el generativo 8/12 (66.7 %), con formato válido en ambos (12/12). Con la métrica estricta (categoría y prioridad) gana el tradicional, pero solo por la prioridad: en categoría el generativo acertó 12/12 y el tradicional 11/12. El generativo interpretó los mensajes ambiguos (casos 8 y 9) que las reglas fallaron, y ante las instrucciones inyectadas clasificó como SECURITY y marcó revisión humana. Sus cuatro errores fueron de prioridad, en parte discutibles, como elevar a HIGH un problema de acceso. El tradicional es predecible y barato, pero rígido: depende de palabras clave y falla ante expresiones no previstas. Con solo 12 casos, y reglas escritas conociendo ese tipo de casos, la diferencia no es concluyente; un enfoque híbrido (reglas más modelo, con revisión humana en seguridad) sería más robusto.

## Evidencias

La carpeta [`evidence/`](evidence/) contiene:

- `compare-output.txt`: salida completa de `npm run compare`.
- `api-traditional.json` y `api-generative.json`: respuestas de la API en cada modo.
- `api-injection.json`: intento de inyección contra el motor generativo.
- `api-error-400.json`: validación de una solicitud inválida.
- Capturas de pantalla de la ejecución.
- `api-extraction-attempt.json`: intento de extraer el prompt y la API key (sin filtración).