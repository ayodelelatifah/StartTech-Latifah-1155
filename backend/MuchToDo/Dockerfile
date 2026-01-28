# Build Stage
FROM golang:1.24-alpine AS builder

# Set working directory
WORKDIR /app

# Copy mod files from the context (which is backend/MuchToDo)
COPY go.mod go.sum ./
RUN go mod download

# Copy all source files from the context into /app
COPY . .

# Explicitly build the main.go file. 
# If your main file is named something else (e.g., server.go), change 'main.go' to that name.
RUN CGO_ENABLED=0 GOOS=linux go build -o main .

# Run Stage
FROM alpine:latest
RUN apk --no-cache add ca-certificates
WORKDIR /root/
COPY --from=builder /app/main .
EXPOSE 8080
CMD ["./main"]