# Stage 1: Build Stage
FROM node:14-alpine AS build
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
# Stage 2: Production Stage
FROM node:14-alpine AS production
WORKDIR /app
COPY --from=build /app .
RUN addgroup -S nodeuser && adduser -S -G nodeuser nodeuser
USER nodeuser
EXPOSE 3000
HEALTHCHECK --interval=30s --timeout=5s CMD wget -q http://localhost:3000/health -O /dev/null || exit 1
ENV NODE_ENV=production
ENTRYPOINT ["node"]
CMD ["app.js"]
