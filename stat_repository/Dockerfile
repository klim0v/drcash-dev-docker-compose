FROM golang:1.12.1-alpine as builder

RUN apk update && apk upgrade && \
    apk add --no-cache bash git openssh

ADD ./ssh_key ssh_key

RUN mkdir -p ~/.ssh && \
    cp ./ssh_key/* ~/.ssh && \
    chmod 0600 -R ~/.ssh && \
    echo "StrictHostKeyChecking no " > ~/.ssh/config && \
    echo -e "[url \"git@github.com:\"]\n\tinsteadOf = https://github.com/" > ~/.gitconfig

RUN git clone git@gitlab.com:dr-code/stat-repository.git

WORKDIR stat-repository

RUN go build main.go

FROM alpine:3.9

WORKDIR /app

RUN apk add --no-cache ca-certificates

COPY --from=builder /go/stat-repository/main /app/app
#todo: add migrations "COPY --from=builder /go/stat-repository/migrations /app/migrations"

#create table visits
#(
#	event_date Date,
#	created_at DateTime,
#	uuid FixedString(36),
#	unique_uuid FixedString(36),
#	previous_uuid FixedString(36),
#	previous_template_id Int32,
#	action_type String,
#	template_type Int32,
#	template_id Int32,
#	stream_code String,
#	stream_aff_user_id Int32,
#	stream_adv_user_id Int32,
#	stream_geo_code Array(String),
#	stream_offer_id Int32,
#	stream_currency FixedString(3),
#	location_ip String,
#	location_geo_code String,
#	referrer_host String,
#	device_type Int32,
#	device_os_name String,
#	device_os_version String,
#	device_browser_name String,
#	device_hardware_brand String,
#	device_browser_version String,
#	sub1 String,
#	sub2 String,
#	sub3 String,
#	sub4 String,
#	sub5 String
#)
#engine = MergeTree() PARTITION BY event_date ORDER BY (stream_currency, stream_aff_user_id, uuid) SETTINGS index_granularity = 8192;
#
#create table orders
#(
#	event_date Date,
#	event_time Int64,
#	order_created_at DateTime,
#	order_uuid FixedString(36),
#	order_type Int32,
#	order_visit_uuid FixedString(36),
#	order_client_uuid FixedString(36),
#	order_stream_code String,
#	order_offer_id Int32,
#	order_user_id Int32,
#	order_status Int32,
#	order_sum Float64,
#	order_payment Float64,
#	order_currency FixedString(3),
#	order_geo_code Array(String),
#	order_error_reason String,
#	order_pills Int32,
#	order_comment String,
#	order_adv_user_id Int32,
#	order_expired_hold Int64,
#	visit_created_at DateTime,
#	visit_unique_uuid FixedString(36),
#	visit_previous_uuid FixedString(36),
#	visit_previous_template_id Int32,
#	visit_action_type String,
#	visit_template_type Int32,
#	visit_template_id Int32,
#	visit_location_ip String,
#	visit_location_geo_code String,
#	visit_referrer_host String,
#	visit_device_type Int32,
#	visit_device_os_name String,
#	visit_device_os_version String,
#	visit_device_browser_name String,
#	visit_device_hardware_brand String,
#	visit_device_browser_version String,
#	client_name String,
#	client_surname String,
#	client_phone String,
#	client_phone_e164 String,
#	client_phone_geo_code String,
#	client_ip String,
#	sub1 String,
#	sub2 String,
#	sub3 String,
#	sub4 String,
#	sub5 String
#)
#engine = ReplacingMergeTree(event_time) PARTITION BY event_date ORDER BY (order_currency, order_user_id) SETTINGS index_granularity = 8192;



CMD ["/app/app"]