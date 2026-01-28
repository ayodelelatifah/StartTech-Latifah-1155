# --- Stage 1: Build Stage ---
FROM golang:1.24-alpine AS builder

# Set the working directory inside the container
WORKDIR /app

# Copy dependency files from the root context path
COPY backend/MuchToDo/go.mod backend/MuchToDo/go.sum ./

# Download all dependencies
RUN go mod download

# Copy the entire backend folder content into /app
COPY backend/MuchToDo/ .

# The recursive build command (./...) finds the main package automatically
RUN CGO_ENABLED=0 GOOS=linux go build -o main ./...

# --- Stage 2: Run Stage ---
FROM alpine:latest

# Add certificates for secure external communication
RUN apk --no-cache add ca-certificates

WORKDIR /root/

# Copy the compiled binary from the builder stage
COPY --from=builder /app/main .

# Expose the application port
EXPOSE 8080

# Run the application
CMD ["./main"]