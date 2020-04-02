FROM smartcontract/cache:0.0.1-1585787725

ENV PATH /chainlink/node_modules/.bin:$PATH

ADD . .

RUN yarn install
RUN yarn setup:explorer
RUN yarn workspace @chainlink/explorer-client build

FROM node:10.16.3-alpine

WORKDIR /app

COPY --from=0 /chainlink/package.json package.json
COPY --from=0 /chainlink/yarn.lock yarn.lock
COPY --from=0 /chainlink/node_modules node_modules
COPY --from=0 /chainlink/explorer/node_modules explorer/node_modules
COPY --from=0 /chainlink/explorer/package.json explorer/package.json
COPY --from=0 /chainlink/explorer/src explorer/src
COPY --from=0 /chainlink/explorer/tsconfig.json explorer/tsconfig.json
COPY --from=0 /chainlink/tsconfig.cjs.json ./
COPY --from=0 /chainlink/explorer/ormconfig.json explorer/ormconfig.json
COPY --from=0 /chainlink/explorer/client/public explorer/client/public
COPY --from=0 /chainlink/explorer/client/build explorer/client/build
COPY --from=0 /chainlink/styleguide styleguide
COPY --from=0 /chainlink/tools/json-api-client tools/json-api-client
COPY --from=0 /chainlink/tools/redux tools/redux

ENV NODE_ENV production
ENTRYPOINT [ "yarn", "workspace", "@chainlink/explorer", "run", "prod" ]
