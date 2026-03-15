FROM node:18 AS build

WORKDIR /usr/local/app

COPY package.json package-lock.json ./
COPY angular.json tsconfig.json tsconfig.app.json tsconfig.spec.json ./
COPY src/ ./src/
RUN npm ci
RUN npm run build

FROM nginx:alpine

RUN addgroup --system appgroup && adduser --system --ingroup appgroup appuser

COPY nginx.conf /etc/nginx/conf.d/default.conf
COPY --from=build /usr/local/app/dist/bobapp /usr/share/nginx/html

USER appuser
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
