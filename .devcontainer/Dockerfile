FROM golang:1.24-alpine3.22

RUN apk add -U tzdata
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN sed -i 's/https/http/' /etc/apk/repositories
RUN apk add --no-cache git openssl ca-certificates

ARG cert_location=/usr/local/share/ca-certificates

# Get certificate from "github.com"
RUN openssl s_client -showcerts -connect github.com:443 </dev/null 2>/dev/null|openssl x509 -outform PEM > ${cert_location}/github.crt
# Get certificate from "proxy.golang.org"
RUN openssl s_client -showcerts -connect proxy.golang.org:443 </dev/null 2>/dev/null|openssl x509 -outform PEM >  ${cert_location}/proxy.golang.crt
# Update certificates

RUN update-ca-certificates


RUN go install github.com/fullstorydev/grpcurl/cmd/grpcurl@latest
