FROM golang:1.22-alpine AS builder

WORKDIR /app

# hadolint ignore=DL3018

RUN apk add --no-cache git

COPY go.mod go.sum ./

RUN go mod download

COPY . .

RUN CGO_ENABLED=0 GOOS=linux go build -ldflags="-s -w" -o devboard-backend .

FROM alpine:3.20

RUN addgroup -S appgroup && adduser -S appuser -G appgroup

WORKDIR /app

COPY --from=builder /app/devboard-backend .

RUN chown appuser:appgroup /app/devboard-backend

USER appuser

EXPOSE 8080

CMD ["./devboard-backend"]