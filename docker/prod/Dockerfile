# syntax=docker/dockerfile:1

ARG NODE_VERSION=21.7.1

FROM node:${NODE_VERSION}-alpine as builder

WORKDIR /home/node/app

COPY package.json package-lock.json ./
RUN npm ci

COPY . .
RUN npm run build

FROM node:${NODE_VERSION}-alpine as runner

WORKDIR /home/node/app

ENV NODE_ENV=production

COPY --from=builder /home/node/app ./

# Ensure correct ownership
RUN chown -R node:node /home/node/app

USER node

# Create node_modules directory if it doesn't exist
RUN mkdir -p /home/node/app/node_modules

# Ensure correct permissions for node_modules
RUN chown -R node:node /home/node/app/node_modules

RUN npm ci --omit=dev

EXPOSE 5504

CMD ["node", "server.js"]
