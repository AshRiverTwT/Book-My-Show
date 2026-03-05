FROM node:18

WORKDIR /app

ENV NODE_OPTIONS=--openssl-legacy-provider
ENV PORT=3000

COPY package.json package-lock.json ./

RUN npm install postcss@8.4.21 postcss-safe-parser@6.0.0 --legacy-peer-deps \
 && npm install \
 && npm install -g serve

COPY . .

RUN npm run build

EXPOSE 3000

CMD ["serve","-s","build","-l","3000"]
