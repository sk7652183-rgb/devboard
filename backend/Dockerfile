# Build stage: use a specific version for reproducibility
FROM dhi.io/golang:1.22-alpine AS builder

WORKDIR /app

# Copy go mod files and download dependencies
COPY go.mod go.sum ./
RUN go mod download

# Copy source code and build the binary
COPY . .
RUN CGO_ENABLED=0 go build -o devboard-backend .

# Final runtime stage: use a minimal, specific image
FROM dhi.io/static:debian13

WORKDIR /app

# Copy the built binary from the builder stage
COPY --from=builder /app/devboard-backend ./devboard-backend

# Set permissions (optional, if needed)
# RUN chmod 0755 ./devboard-backend

# Expose only the required port
EXPOSE 8080

# Run as non-root user for security
USER nonroot

# Use exec form for proper signal handling
ENTRYPOINT ["./devboard-backend"]