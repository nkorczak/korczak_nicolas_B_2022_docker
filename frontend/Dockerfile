FROM node:14-alpine
WORKDIR /app
ENV PATH /app/node_modules/.bin:$PATH
COPY . .
RUN npm install
EXPOSE 8081
CMD ["npm", "start"]
