# Stage 1: Build the Go app
FROM golang:1.23 AS builder

# Set the working directory
WORKDIR /app

# Copy go.mod and go.sum files
COPY go.mod go.sum ./

# Download all dependencies
RUN go mod download

# Copy the source code
COPY . .

# Build the Go application with static linking to ensure portability
RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -o hellogo .

# Stage 2: Create a minimal image for running the Go app
FROM alpine:latest

# Install certificates for HTTPS support
RUN apk add --no-cache ca-certificates

# Set working directory
WORKDIR /

# Copy the compiled binary from the builder stage
COPY --from=builder /app/hellogo .

# Set environment variables
ENV SERVICE_NAME=goGinApp \
    INSECURE_MODE=true \
    OTEL_EXPORTER_OTLP_ENDPOINT=localhost:4317

# Expose the application port
EXPOSE 8080

# Command to run the application
CMD ["./hellogo"]
