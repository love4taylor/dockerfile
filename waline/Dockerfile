# https://github.com/nodejs/LTS
FROM node:lts AS build
WORKDIR /app
ENV NODE_ENV production
RUN set -eux; \
	# npm config set registry https://registry.npm.taobao.org; \
	npm install --production --silent @waline/vercel@1.7.0; \
	sed -i "s@module.exports = {@module.exports = {\n  host: '0.0.0.0',@g" node_modules/@waline/vercel/src/config/config.js

FROM node:lts-buster-slim
WORKDIR /app
ENV TZ Asia/Shanghai
ENV NODE_ENV production
COPY --from=build /app .
EXPOSE 8360
CMD ["node", "node_modules/@waline/vercel/vanilla.js"]
