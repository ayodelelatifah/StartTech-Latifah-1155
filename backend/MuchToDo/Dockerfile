FROM golang:1.24-alpine AS builder
WORKDIR /app

# Copy the mod files from the current context
COPY go.mod go.sum ./
RUN go mod download

# Copy everything from the current context into /app
COPY . .

# Build the application
RUN CGO_ENABLED=0 GOOS=linux go build -o main .

FROM alpine:latest
RUN apk --no-cache add ca-certificates
WORKDIR /root/
COPY --from=builder /app/main .
EXPOSE 8080
CMD ["./main"]