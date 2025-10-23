# Stage 1: Build stage
FROM golang:1.20 AS builder

# Set working directory
WORKDIR /app

# Initialize a Go module
COPY main.go .
RUN go mod init guestbook

# Download dependencies
RUN go get github.com/codegangsta/negroni \
    && go get github.com/gorilla/mux \
    && go get github.com/xyproto/simpleredis/v2

# Build the Go binary
RUN go build -o main main.go

# Stage 2: Runtime stage
FROM ubuntu:22.04

COPY --from=builder /app/main /main
ADD public/index.html /public/index.html
ADD public/script.js /public/script.js
ADD public/style.css /public/style.css

EXPOSE 3000
CMD ["/main"]

