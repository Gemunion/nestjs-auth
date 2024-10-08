# JWKS based authorization for Nest.js

JWKS stands for JSON Web Key Sets. This is a set of keys containing the public keys used to verify any JWT issued by the authorization server

This type of authorization feats the best if you are going to have only one Authorization provider (like Auth0) with multiple accounts

## Installation

I assume you have nodejs, yarn/npm and postgres

First you have to download dependencies

```bash
npm i
```

Then check config in

```bash
nano .env
```

and start in watch mode

```bash
npm run start
```

or in production mode

```bash
npm run build
npm run prod
```

Change email for yours in `src/migrations/1563804021014-seed-users.ts`

## Docker

Otherwise, you can use docker

```shell script
docker-compose up --build
```

## Usage

Go to https://developers.google.com/oauthplayground
select Google OAuth2 API v2 and get id_token

Put this id_token in the header of each of your subsequent requests

```bash
curl \
http://localhost:3000/users/profile \
-H "Authorization: Bearer eyJhbGciOiJSUzI1NiIsImtpZCI6ImI2ZjhkNTVkYTUzNGVhOTFjYjJjYjAwZTFhZjRlOGUwY2RlY2E5M2QiLCJ0eXAiOiJKV1QifQ.eyJpc3MiOiJodHRwczovL2FjY291bnRzLmdvb2dsZS5jb20iLCJhenAiOiI0MDc0MDg3MTgxOTIuYXBwcy5nb29nbGV1c2VyY29udGVudC5jb20iLCJhdWQiOiI0MDc0MDg3MTgxOTIuYXBwcy5nb29nbGV1c2VyY29udGVudC5jb20iLCJzdWIiOiIxMTM3ODk4MTExNTcxNTY1MDUwNTEiLCJlbWFpbCI6InRyZWpndW5AZ21haWwuY29tIiwiZW1haWxfdmVyaWZpZWQiOnRydWUsImF0X2hhc2giOiJJMHRQTjNUSmhjNnZOTHZRNm9RU29RIiwibmFtZSI6IlRyZWogR3VuIiwicGljdHVyZSI6Imh0dHBzOi8vbGgzLmdvb2dsZXVzZXJjb250ZW50LmNvbS9hLS9BT2gxNEdqU2FEeWNqWVBLWVhDc3hEUmJ3cy1DU3plU3pzTXpmUjNDb0VOMj1zOTYtYyIsImdpdmVuX25hbWUiOiJUcmVqIiwiZmFtaWx5X25hbWUiOiJHdW4iLCJsb2NhbGUiOiJlbiIsImlhdCI6MTYyNTcxOTk2NywiZXhwIjoxNjI1NzIzNTY3fQ.Hj5SZlufazinTpuS-OAZX-_Lm-Ou28kEy4cVGO8XwVV-AITD77eIiXFV727f6WJMlgTGA8vMZqqBqI85DTSZyl9Xc7pXKkBY0sssm1YFt_t8OopUC2fqst92z16yPnxhTlTwtFyhgBRoLEzLG7Jbb7IWcoCbqvu8XY70YlpnnLQWio-zlRiPtoC0gq64uMxs5Z64kqIg04WM8iZTEE3-av807HP-SPoS4M9PsUKkk8whDatR28bLxMbfsOEBJKwPz7Ks2R-G9onM1t7hGM745sK021XqGD4a7yz4epxB-7HmTV28NdVJpL2SCYcA0iXuZT2FYuyGPYn7sCCkOSFYgQ"

```

This will return your profile

```json
{ "id": 1, "email": "trejgun@gmail.com", "roles": ["ADMIN"] }
```

```bash
curl \
http://localhost:3000/users \
-H "Authorization: Bearer eyJhbGciOiJSUzI1NiIsImtpZCI6ImYwOTJiNjEyZTliNjQ0N2RlYjEwNjg1YmI4ZmZhOGFlNjJmNmFhOTEiLCJ0eXAiOiJKV1QifQ.eyJpc3MiOiJodHRwczovL2FjY291bnRzLmdvb2dsZS5jb20iLCJhenAiOiI0MDc0MDg3MTgxOTIuYXBwcy5nb29nbGV1c2VyY29udGVudC5jb20iLCJhdWQiOiI0MDc0MDg3MTgxOTIuYXBwcy5nb29nbGV1c2VyY29udGVudC5jb20iLCJzdWIiOiIxMTM3ODk4MTExNTcxNTY1MDUwNTEiLCJlbWFpbCI6InRyZWpndW5AZ21haWwuY29tIiwiZW1haWxfdmVyaWZpZWQiOnRydWUsImF0X2hhc2giOiJsanlBLUhEdlBnLVF1T0NNazBxNzVnIiwibmFtZSI6IlRyZWogR3VuIiwicGljdHVyZSI6Imh0dHBzOi8vbGgzLmdvb2dsZXVzZXJjb250ZW50LmNvbS9hLS9BT2gxNEdqU2FEeWNqWVBLWVhDc3hEUmJ3cy1DU3plU3pzTXpmUjNDb0VOMj1zOTYtYyIsImdpdmVuX25hbWUiOiJUcmVqIiwiZmFtaWx5X25hbWUiOiJHdW4iLCJsb2NhbGUiOiJlbiIsImlhdCI6MTYwNDg5MzU4MiwiZXhwIjoxNjA0ODk3MTgyfQ.o9Isjc_gogof91_GZEyDY-04KOUXT6O4CkRjA01ZIVC_3fbEKyBoR2Zabwu8UEH2GeatHtAbp_6uitO2RF4P4AJAlq0gvHkjb4GJXIfkuX2CrTZa5BG-A3MaiOtfW6L4_jSh7mpihJDGQ8VQKIuJWqWiaF2ukCBdRs6HtCyMdw1HEQnRd1oww8Y03oqy0VYG8BFj28rAkfMoDCwd_vvviD8mBdt5-fZnZ0MGnMZ6dv7nV-07FYRo2_9FrxEvUFfbAeIdaQcejQ25YXAMl2fMUT4b4b0pk2IOaFv4M0EjOCnWZ09LPoWBE7xzjW72NMR9fRvFbuH-NjWjzvC4vh63ow"
```

This will return a list of users

```json
{ "list": [{ "id": 1, "email": "trejgun@gmail.com", "roles": ["ADMIN"] }], "count": 1 }
```
