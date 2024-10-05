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

# Build the Go application
RUN go build -o hellogo .

# Stage 2: Create a minimal image for running the Go app
FROM gcr.io/distroless/base-debian10

# Set working directory
WORKDIR /

# Copy the compiled binary from the builder stage
COPY --from=builder /app/hellogo .

# Expose the application port
EXPOSE 8080

# Command to run the application
CMD ["./hellogo"]
