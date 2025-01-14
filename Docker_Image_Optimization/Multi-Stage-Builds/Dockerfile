# ----------------------------------------- Multi-Stage Builds --------------------------------------------------
# Stage 1
# In the first stage all the things have been copied will be deleted at the the Second stage
FROM node:22.13.0-alpine AS builder

WORKDIR /build

# If something change in the package.json or package-lock.json file so all the down stream layers will be cached invalidate.
COPY package.json package.json
COPY package-lock.json package-lock.json  

RUN npm install

COPY . .

RUN npm run build 

CMD ["npm", "start"]


# Stage 2
# The docker will execute this stage:runner and only those things will be remaining that have been copied from the builder stage. 
FROM node:22.13.0-alpine AS runner

WORKDIR /app

# Copy from our builder stage and define what should be copied in the 1st argument and paste it to the /app root working directory as the 2nd argument  
COPY --from=builder /build/node_modules node_modules  
COPY --from=builder /build/package.json package.json
COPY --from=builder /build/package-lock.json package-lock.json
COPY --from=builder /build/dist/ dist/

CMD ["npm", "start"]