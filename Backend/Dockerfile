FROM node:20

# Install netcat for entrypoint wait check
RUN apt-get update && apt-get install -y netcat-openbsd && rm -rf /var/lib/apt/lists/*
RUN apt-get update && \
    apt-get install -y netcat-openbsd postgresql-client && \
    rm -rf /var/lib/apt/lists/*
# Create app directory
WORKDIR /app

# Install app dependencies
COPY package*.json ./
RUN npm install

# Copy app source
COPY . .

# Generate Prisma client
RUN npx prisma generate


# Copy & register the entrypoint
COPY entrypoint.sh .

RUN sed -i 's/\r$//' entrypoint.sh && chmod +x entrypoint.sh

# Expose backend port
EXPOSE 3001

# Run the entrypoint (which seeds once then starts)
CMD ["sh","./entrypoint.sh"]