FROM node:16-alpine


ARG DATABASE_URL # you could give this a default value as well
ENV DATABASE_URL=$DATABASE_URL

ARG DATABASE_TYPE # you could give this a default value as well
ENV DATABASE_TYPE=$DATABASE_TYPE

ARG JWT_SECRET # you could give this a default value as well
ENV JWT_SECRET=$JWT_SECRET

ARG IPREGISTRY_API_KEY # you could give this a default value as well
ENV IPREGISTRY_API_KEY=$IPREGISTRY_API_KEY

# Dependencies
RUN apk add --no-cache dos2unix
RUN npm install -g nodemon prisma

# Start script
COPY --chown=node:node ./docker-start.sh /entrypoint.sh
RUN dos2unix /entrypoint.sh
RUN chmod +x /entrypoint.sh

# Create workdir
WORKDIR /home/node/app

# Install dependencies
COPY package.json .
RUN yarn install

# Copy the rest of the app
COPY . .

# Generate prisma
RUN prisma generate

# Set permissions
RUN chown -R node:node .
USER node

# Environment
ENV NODE_ENV production
ENV PORT 4000
EXPOSE 4000

ENTRYPOINT ["/bin/sh", "/entrypoint.sh"]