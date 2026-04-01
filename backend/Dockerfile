FROM python:3.12-slim
WORKDIR /app

# Dependencies zuerst (besseres Layer-Caching)
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Source wird per Volume gemountet (live reload)
COPY . .

EXPOSE 8000

# --reload damit Änderungen sofort aktiv sind
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000", "--reload"]
