# segmentAI

Sube tu base de clientes, segméntala con clustering y **conversa con los segmentos usando IA** para diseñar campañas de marketing.

Proyecto creado para una hackatón.

## Qué hace

1. **Datos**: subes un CSV o Excel, transaccional (una fila por compra) o tabla de clientes (una fila por cliente), o usas el dataset de ejemplo.
2. **Procesamiento**: la IA identifica el ID de cliente, las fechas, los montos y los datos personales (PII). Después se calculan features RFM por cliente y se corre KMeans, eligiendo k automáticamente por silhouette.
3. **Clusters**: gráfico de dispersión (gasto vs frecuencia) con los segmentos. La IA propone un nombre para cada uno y puedes editarlo.
4. **Exploración**: ficha de cada segmento con sus rasgos, reglas legibles (`monetary > 938 Y recency_days ≤ 91`), el cliente más representativo, ventas, frecuencia de compra, productos principales y estrategias sugeridas.
5. **Campaña**: eliges un segmento y defines con la IA el objetivo, el canal y el mensaje, y exportas la audiencia.

Desde cualquier paso puedes **preguntarle a la IA**, en dos modos:

- **Analista**: responde con datos reales usando herramientas (SQL sobre DuckDB, comparación de clusters, explicación de clientes, listas exportables).
- **Persona**: conversas con un cliente simulado del segmento.

**Privacidad por diseño:** el LLM nunca ve filas completas ni columnas PII. Solo recibe metadatos, agregados y las features de un cliente puntual cuando se pide explicarlo. Las listas de clientes con datos de contacto van directo a la interfaz.

## Stack

| Parte | Tecnología |
|---|---|
| Frontend (`frontend/`) | React 18, Vite, react-router-dom |
| Backend (`backend/`) | Python 3.12, FastAPI, pandas, scikit-learn, DuckDB + sqlglot |
| IA | Gemini vía Vertex AI (`google-genai`) |

## Cómo empezar

```bash
git clone https://github.com/AndresFaundez/segmentAI.git
cd segmentAI
```

### Backend

```bash
cd backend
python3 -m venv .venv && .venv/bin/pip install -e '.[dev]'   # o: uv sync --extra dev
cp .env.example .env                                         # configura las credenciales de Gemini
.venv/bin/python scripts/make_synthetic.py                   # dataset de ejemplo (opcional)
.venv/bin/uvicorn app.main:app --reload                      # http://localhost:8000/docs
```

Sin un LLM configurado, todo funciona con fallbacks heurísticos excepto el chat, que responde 503. Las credenciales, los endpoints, los tests y la arquitectura están documentados en [`backend/README.md`](backend/README.md).

### Frontend

```bash
cd frontend
npm install
cp .env.example .env.local   # solo si el backend no está en http://localhost:8000
npm run dev                  # http://localhost:5173
```

## Estructura

```
backend/     API de clustering, perfilado de segmentos y agente de chat
frontend/    interfaz en 5 pasos: Datos → Procesamiento → Clusters → Exploración → Campaña
AGENTS.md    contexto y convenciones para agentes de IA que trabajan en el repo
```

## Equipo

- Julio Yáñez
- Andrés Faúndez
