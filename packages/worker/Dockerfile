# 빌드 스테이지
FROM node:20 AS build

RUN apt-get update && apt-get install -y \
    g++ \
    build-essential

WORKDIR /app

# 루트 package.json만 복사 및 설치
COPY package*.json ./
RUN npm install --legacy-peer-deps
RUN npm install @rollup/rollup-linux-x64-gnu --save-dev

# worker 폴더만 복사
COPY packages/worker ./packages/worker

# worker 빌드
RUN npm run build:worker

# 프로덕션 스테이지
FROM nginx:alpine

RUN apk update && apk add --no-cache ca-certificates curl
RUN rm -rf /usr/share/nginx/html/*

COPY --from=build /app/packages/worker/dist /home/ec2-user/frontend/worker/dist

EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
