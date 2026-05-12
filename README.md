# Scheduler Ops Dashboard

> Monitoramento, aprovacao e acompanhamento de jobs assincronos com fila, historico e reprocessamento.

![React](https://img.shields.io/badge/React-18-61dafb?logo=react)
![Django](https://img.shields.io/badge/Django-5-092e20?logo=django)
![Celery](https://img.shields.io/badge/Celery-5-37814a?logo=celery)
![Redis](https://img.shields.io/badge/Redis-7-ff4438?logo=redis)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-15-316192?logo=postgresql)

---

## Contexto

Operacoes recorrentes ficavam invisiveis: tarefas eram disparadas manualmente, sem fila, historico ou retentativa controlada. Falhas deixavam de ser caixa-preta: operador enxerga fila, motivo do erro e caminho de reproprocessamento.

## Solucao

Dashboard para criar, aprovar e acompanhar jobs assincronos com status, logs e workers separados da API.

---

## Arquitetura

```text
React Dashboard  ->  Django API  ->  Redis Queue  ->  Workers Celery
                                         ->  PostgreSQL
```

---

## Funcionalidades

- Criacao e aprovacao de jobs assincronos
- Fila observavel com status em tempo real
- Logs por job com motivo de erro
- Reprocessamento sem duplicar efeitos
- Retentativas com limite configuravel
- Jobs idempotentes
- Separacao entre comando e execucao
- Admin panel Django

---

## Decisoes Tecnicas

- **Jobs idempotentes**: execucao sem efeitos colaterais duplicados
- **Retentativas com limite**: falhas sao tentadas N vezes antes de falhar
- **Separacao comando/execucao**: API recebe o pedido, Celery executa
- **Logs estruturados**: cada job tem historico completo de execucao
- **Redis como broker**: fila de mensagens entre API e workers

---

## Metricas

| Indicador | Meta |
|-----------|------|
| Jobs em lote | 1.000 |
| Taxa de erro | abaixo de 1% |
| Reprocessamento | sem duplicar efeitos |

---

## Como rodar localmente

### Pre-requisitos

- Python >= 3.10
- Node.js >= 18
- Redis
- PostgreSQL

### Backend (Django + Celery)

```bash
cd backend
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
python manage.py migrate
python manage.py runserver
```

```bash
# Celery worker
python -m celery -A scheduler worker -l info
```

### Frontend (React)

```bash
cd frontend
npm install
npm run dev
```

---

## Variaveis de ambiente

```env
# Backend
DEBUG=True
SECRET_KEY=sua-chave-secreta
DATABASE_URL=postgresql://user:pass@localhost:5432/scheduler_db
REDIS_URL=redis://localhost:6379/0

# Frontend
VITE_API_URL=http://localhost:8000/api
```

---

## Estrutura do projeto

```
scheduler-ops-dashboard/
├── backend/           # Django API + Celery Workers
│   ├── api/          # Endpoints REST
│   ├── scheduler/    # Tarefas Celery
│   └── admin/        # Django Admin
├── frontend/         # React Dashboard
│   ├── src/
│   └── public/
├── .gitignore
├── .env.example
├── LICENSE
└── README.md
```

---

## Autor

Desenvolvido por **Gabriel Galvim**

[GitHub](https://github.com/Gabrielgalvimdev) | [LinkedIn](https://linkedin.com/in/gabriel-galvim)

---

*Dashboard operacional completo para monitoramento de jobs assincronos.*
