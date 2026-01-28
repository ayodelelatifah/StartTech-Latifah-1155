# Build Stage
FROM golang:1.24-alpine AS builder
WORKDIR /app

# Point to the specific folder paths from the root
COPY backend/MuchToDo/go.mod backend/MuchToDo/go.sum ./
RUN go mod download

# Copy everything from that specific folder into /app
COPY backend/MuchToDo/ .

# Now build the local package
RUN CGO_ENABLED=0 GOOS=linux go build -o main .

# Run Stage
FROM alpine:latest
RUN apk --no-cache add ca-certificates
WORKDIR /root/
COPY --from=builder /app/main .
EXPOSE 8080
CMD ["./main"]