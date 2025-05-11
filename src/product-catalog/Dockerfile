FROM golang:1.24-alpine AS builder

WORKDIR /usr/src/app/

# Use Go build cache for dependencies
RUN --mount=type=cache,target=/go/pkg/mod \
    --mount=type=cache,target=/root/.cache/go-build \
    mkdir -p /root/.cache/go-build

# Copy go.mod and go.sum
COPY go.mod go.sum ./

# Download dependencies
RUN go mod download

# Copy the rest of the source code
COPY . .

# Build the app
RUN go build -o product-catalog .

####################################

FROM alpine AS release

WORKDIR /usr/src/app/

# Copy products directory
COPY ./products/ ./products/

# Copy the built binary from builder
COPY --from=builder /usr/src/app/product-catalog ./

# Set environment variable (fixed format)
ENV PRODUCT_CATALOG_PORT=8088

ENTRYPOINT ["./product-catalog"]
