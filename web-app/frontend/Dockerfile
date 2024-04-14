# stage1 as builder
FROM node:16.13.0-alpine as builder

# copy the package.json to install dependencies
COPY package.json package-lock.json ./

# Install the dependencies and make the folder
RUN npm install 
RUN mkdir /react-ui && mv ./node_modules ./react-ui

WORKDIR /react-ui

COPY . .

# Build the project and copy the files
RUN npm run build


# FROM nginx:alpine
FROM nginx:stable-alpine
## Remove default nginx index page
# RUN rm -rf /usr/share/nginx/html/*
# RUN rm -rf /etc/nginx/conf.d/default.conf
# COPY ./nginx.conf /etc/nginx/conf.d
COPY --from=builder /react-ui/build/ /usr/share/nginx/html
COPY nginx.conf /etc/nginx/conf.d/default.conf

# Copy from the stage 1

# RUN chmod +x /usr/sbin/nginx
EXPOSE 80

ENTRYPOINT ["nginx", "-g", "daemon off;"]