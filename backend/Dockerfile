FROM golang:1.24.3 AS builder
WORKDIR /app
COPY . .
RUN go mod download && CGO_ENABLED=0 go build -o backend ./cmd/api/

FROM alpine:3.21.3
RUN adduser -D user
USER user
COPY --from=builder /app/backend /bin/backend
ENTRYPOINT ["/bin/backend"]
