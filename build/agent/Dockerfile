# syntax=docker/dockerfile:1
FROM golang:1.17
WORKDIR /build
COPY . .
ENV CGO_ENABLED=0 
ENV GOOS=linux

RUN cd cmd/agent;go build -o /runner -mod mod -a .

# using debian based image as alpine is not supported by Cypress yet
# https://github.com/cypress-io/cypress/issues/419 

FROM node:17
RUN apt-get update && apt-get install -y libgtk2.0-0 \
    libgtk-3-0 \
    libgbm-dev \
    libnotify-dev \
    libgconf-2-4 \
    libnss3 \
    libxss1 \
    libasound2 \
    libxtst6 \
    xauth \
    xvfb \
	git
 
RUN npm install -g cypress browserslist@latest

WORKDIR /root/
COPY --from=0 /runner /bin/runner
ENTRYPOINT ["/bin/runner"]
