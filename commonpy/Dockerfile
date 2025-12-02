# Use uma imagem Python estável como base
FROM python:3.11-slim

# Define o diretório de trabalho dentro do container
WORKDIR /app

# Instala as ferramentas de build e dependências de sistema necessárias
# para compilar pacotes Python que possuem código C/C++.
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        build-essential \
        python3-dev \
    && rm -rf /var/lib/apt/lists/*

# Copia os arquivos de requisitos primeiro para aproveitar o cache de camadas do Docker
COPY requirements.txt requirements-dev.txt ./

# Instala o 'hatch' (necessário para rodar os testes conforme o Makefile) e todas as dependências
# O requirements-dev.txt inclui as dependências de produção.
RUN pip install --no-cache-dir hatch && \
    pip install --no-cache-dir -r requirements-dev.txt

# Copia o restante do código da aplicação
COPY . .

# Define o comando padrão para rodar os testes usando 'hatch', garantindo que o ambiente está funcional
CMD ["hatch", "run", "test"]
