# Build the cluster binary
FROM golang:1.10.3 as builder

# Copy in the go src
WORKDIR /go/src/sigs.k8s.io/cluster-api-provider-vsphere
COPY pkg/    pkg/
COPY cmd/    cmd/
COPY vendor/ vendor/

# Build
RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -a -o clusterctl sigs.k8s.io/cluster-api-provider-vsphere/cmd/clusterctl

# Copy clusterctl into a thin image
FROM debian:stretch-slim
WORKDIR /root/

RUN apt-get update && apt-get install -y ca-certificates openssh-client

COPY --from=builder /go/src/sigs.k8s.io/cluster-api-provider-vsphere/clusterctl .
ENTRYPOINT ["./clusterctl"]
