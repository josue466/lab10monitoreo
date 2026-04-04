FROM node:18

# Instalar dependencias para Docker
RUN apt-get update && apt-get install -y \
    curl \
    git \
    unzip \
    python3 \
    python3-pip \
    && rm -rf /var/lib/apt/lists/*

# Instalar Docker CLI
RUN curl -fsSL https://get.docker.com -o get-docker.sh \
    && sh get-docker.sh \
    && rm get-docker.sh

# Instalar Docker Compose
RUN curl -L "https://github.com/docker/compose/releases/download/v2.21.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose \
    && chmod +x /usr/local/bin/docker-compose \
    && docker-compose --version

WORKDIR /app
COPY . .

RUN npm install

EXPOSE 3000

HEALTHCHECK --interval=10s --timeout=5s CMD curl -f http://localhost:3000/ || exit 1

CMD ["npm", "start"]