FROM node:20-alpine
WORKDIR /app

# Dependencies zuerst (besseres Layer-Caching)
COPY package*.json ./
RUN npm install

# Source wird per Volume gemountet (live reload)
COPY . .

EXPOSE 5173

# --host 0.0.0.0 damit Nginx den Container erreicht
CMD ["npm", "run", "dev", "--", "--host", "0.0.0.0"]
