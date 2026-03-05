FROM node:18

WORKDIR /app

# Fix OpenSSL issue for Node 18 before build
ENV NODE_OPTIONS=--openssl-legacy-provider
ENV PORT=3000

COPY package.json package-lock.json ./

# Install compatible PostCSS versions
RUN npm install postcss@8.4.21 postcss-safe-parser@6.0.0 --legacy-peer-deps

# Install dependencies
RUN npm install

# Copy project
COPY . .

# Build React app
RUN npm run build

# Install static server
RUN npm install -g serve

EXPOSE 3000

CMD ["serve","-s","build","-l","3000"]
