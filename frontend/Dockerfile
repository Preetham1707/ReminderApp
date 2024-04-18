FROM node:20.11-alpine as react-build

WORKDIR /app

COPY . .

RUN  NODE_ENV=development npm install

EXPOSE 3000

RUN npm run build


# stage-2 : Create the production Image
FROM nginx:latest

COPY --from=react-build /app/build /usr/share/nginx/html

RUN rm /etc/nginx/conf.d/default.conf

COPY nginx/nginx.conf /etc/nginx/conf.d

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
