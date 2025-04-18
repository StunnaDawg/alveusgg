FROM node:22-alpine
ENV PNPM_HOME="/pnpm"
ENV PATH="$PNPM_HOME:$PATH"
WORKDIR /app

## Copy root files
COPY package.json .
COPY pnpm-lock.yaml .
COPY pnpm-workspace.yaml .
COPY patches ./patches

## Copy chatbot files
WORKDIR apps/chatbot
COPY ./apps/chatbot/tsconfig.json .
COPY ./apps/chatbot/package.json .
COPY ./apps/chatbot/.env .

## Install dependencies
RUN npm install -g corepack@latest && corepack enable && corepack install
RUN --mount=type=cache,id=pnpm,target=/pnpm/store pnpm install --prod --frozen-lockfile --filter "." --ignore-scripts

## Copy code
COPY ./apps/chatbot/src/ src/

CMD ["pnpm", "start"]
