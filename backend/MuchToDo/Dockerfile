# --- Stage 1: Build Stage ---
# We use golang:1.24-alpine to satisfy your project's dependency requirements
FROM golang:1.24-alpine AS builder

# Set the working directory inside the container
WORKDIR /app

# Copy the dependency files first (optimizes Docker layer caching)
COPY go.mod go.sum ./

# Download all dependencies
RUN go mod download

# Copy the rest of the source code from backend/MuchToDo
COPY . .

# Compile the application into a binary named 'main'
RUN CGO_ENABLED=0 GOOS=linux go build -o main .

# --- Stage 2: Run Stage ---
# We use a minimal alpine image to run the app, keeping the final size small
FROM alpine:latest

# Install CA certificates in case your app makes HTTPS requests
RUN apk --no-cache add ca-certificates

WORKDIR /root/

# Copy only the compiled binary from the builder stage
COPY --from=builder /app/main .

# Expose the port your application listens on
EXPOSE 8080

# Start the application
CMD ["./main"]