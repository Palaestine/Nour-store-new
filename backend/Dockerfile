
# Stage 1: Dependencies
FROM node:22-slim AS deps

WORKDIR /app

RUN apt-get update && apt-get install -y openssl \
    && rm -rf /var/lib/apt/lists/*

COPY package*.json ./
COPY prisma ./prisma

RUN npm ci
RUN npm prune --omit=dev

# Stage 2: Production
FROM node:22-slim AS production

WORKDIR /app

RUN apt-get update && apt-get install -y openssl \
    && rm -rf /var/lib/apt/lists/*

ENV NODE_ENV=production
ENV PORT=4000

COPY --from=deps /app/node_modules ./node_modules
COPY package*.json ./
COPY prisma ./prisma
COPY src ./src
COPY cloudinary.js ./

EXPOSE 4000

CMD ["npm", "start"]

