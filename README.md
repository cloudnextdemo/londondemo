**index.js**

```Javascript
const express = require('express')
const app = express()
const port = process.env.PORT || 8080

app.get('/', (req, res) => res.send('Hello World!'))

app.listen(port, () => console.log(`Example app listening on port ${port}!`))
```

```Dockerfile
# Use the official lightweight Node.js 12 image.
# https://hub.docker.com/_/node
FROM node:12-slim

# Create and change to the app directory.
WORKDIR /usr/src/app

# Copy application dependency manifests to the container image.
# A wildcard is used to ensure both package.json AND package-lock.json are copied.
# Copying this separately prevents re-running npm install on every code change.
COPY package*.json ./

# Install production dependencies.
RUN npm install --only=production

# Copy local code to the container image.
COPY . ./

# Run the web service on container startup.
CMD [ "npm", "start" ]

```

Build and run locally...
```
docker build . --tag helloworld
docker run -p 8080:8080 helloworld
```

Build in Cloud Build
```
gcloud builds submit --tag gcr.io/$GOOGLE_CLOUD_PROJECT/helloworld
```

Deploy to Cloud Run
```
gcloud run deploy helloworld \
  --image gcr.io/$GOOGLE_CLOUD_PROJECT/helloworld \
  --platform managed \
  --region us-central1 \
  --allow-unauthenticated
```
