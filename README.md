#### Задание 8. ПИС.

Контроллеры:
```js
app.get("/", (_, res: Response) => {
  res.end(JSON.stringify(counter));
});

app.get("/stat", (_, res: Response) => {
  res.end(JSON.stringify(counter++));
});

app.get("/about", (_, res: Response) => {
  const name = "Alex Chirkov";
  const html = `<h3>Hello, ${name}!</h3>`;

  res.writeHead(200, {'Content-Type': 'text/html'});
  res.end(html);
});
```

```dockerfile
FROM node:latest
WORKDIR /app
COPY ["index.ts",  "package.json", "tsconfig.json", ".env", "./"]
RUN npm install
RUN npm run build
CMD npm run start
EXPOSE 8080
```

Использование образа:
```text
docker build -t tektouser/task-8:latest .
docker push tektouser/task-8:latest
docker pull tektouser/task-8:latest
docker run tektouser/task-8:latest
```


C помощью docker-compose запускаем два контейнера (первый с базой, второй – с приложением-счетчиком) с помощью команды:
```text
docker-compose up
```
Внутрь прокидываются переменные окружения из файла .env, счетчик цепляется к базе и производит запросы через pg-promise. База данных – PostgreSQL.

#### CI / CD
GitHub Actions скрипты для автоматической сборки и развертывания приложения присутствуют в соответствующей папке. 
Разворачивание происходит на удаленный арендованный сервер.
