FROM dhi.io/golang:1-alpine AS builder

WORKDIR /app

COPY go.mod go.sum ./
RUN go mod download

COPY . .
RUN CGO_ENABLED=0 go build -o devboard-backend .

FROM dhi.io/golang:1-alpine

WORKDIR /app

COPY --from=builder /app/devboard-backend ./devboard-backend

# Ensure binary is executable
RUN chmod +x ./devboard-backend

# DHI image runs as nonroot by default
USER nonroot

EXPOSE 8080

# Use exec form to ensure signals are forwarded to the binary and that it can be properly shut down
ENTRYPOINT ["./devboard-backend"]