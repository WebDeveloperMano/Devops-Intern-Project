FROM golang:1.24.4

WORKDIR /app
COPY . .

RUN go mod tidy
RUN go build -o app .

EXPOSE 8001
CMD ["./app"]
