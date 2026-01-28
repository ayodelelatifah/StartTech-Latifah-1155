# Build Stage
FROM golang:1.24-alpine AS builder
WORKDIR /app

# Copy dependency files relative to the root context
COPY backend/MuchToDo/go.mod backend/MuchToDo/go.sum ./
RUN go mod download

# Copy your source code into /app
COPY backend/MuchToDo/ .

# The 'magic' recursive build that finds your main.go automatically
RUN CGO_ENABLED=0 GOOS=linux go build -o main ./...

# Run Stage
FROM alpine:latest
RUN apk --no-cache add ca-certificates
WORKDIR /root/
COPY --from=builder /app/main .
EXPOSE 8080
CMD ["./main"]