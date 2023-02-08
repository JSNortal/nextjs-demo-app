FROM node:18.12.1 AS base

WORKDIR /app

COPY package.json ./

RUN npm install

COPY prisma/schema.prisma  ./prisma/

COPY src ./src

COPY tsconfig.json ./

RUN npm run build

COPY prisma/seed.ts  ./prisma/

RUN npm run prisma generate

FROM node:18.12.1 AS prod

WORKDIR /app

COPY --from=base /app/node_modules/ ./node_modules
COPY --from=base /app/package.json ./package.json
COPY --from=base /app/dist ./dist
COPY --from=base /app/prisma ./prisma
COPY --from=base /app/src ./src
COPY --from=base /app/tsconfig* ./


EXPOSE 3001

CMD ["node", "dist/index.js"]
