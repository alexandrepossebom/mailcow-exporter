# mailcow exporter

[Prometheus](https://prometheus.io) exporter for information about a
[mailcow](https://github.com/mailcow/mailcow-dockerized) instance.

## Usage

```bash
# As a docker container
$ docker run -p '9099:9099' thej6s/mailcow-exporter

# Natively
$ ./mailcow-exporter
```

The `/metrics` endpoint requires `host` and `apiKey` URL parameters. `host` is the
hostname of your mailcow instance. `apiKey` should be a readonly API key that can
be generated by logging into the mailcow management interface and navigating to
'Access > API'.

![Visualization of where to find the API Key](./.readme/api-key)

The following prometheus configuration can be used in order to pass these information
to the endpoint:

```yaml
scrape_configs:
  - job_name: 'mailcow'
    static_configs:
      - targets: [ 'mailcow_exporter:9099' ]
    params:
      host: [ 'mail.example.com' ]
      apiKey: [ 'YOUR-APIKEY-HERE' ]
```

## Example metrics

```
# HELP mailcow_mailbox_last_login Timestamp of the last IMAP login for this mailbox
# TYPE mailcow_mailbox_last_login gauge
mailcow_mailbox_last_login{host="mail.example.com",mailbox="foo@bar.com"} 1.599255303e+09
mailcow_mailbox_last_login{host="mail.example.com",mailbox="test@bar.com"} 1.599247706e+09

# HELP mailcow_mailbox_messages Number of messages in the mailbox
# TYPE mailcow_mailbox_messages gauge
mailcow_mailbox_messages{host="mail.example.com",mailbox="foo@bar.com"} 23476
mailcow_mailbox_messages{host="mail.example.com",mailbox="test@bar.com"} 1891

# HELP mailcow_mailbox_quota_allowed Quota maximum for the mailbox in bytes
# TYPE mailcow_mailbox_quota_allowed gauge
mailcow_mailbox_quota_allowed{host="mail.example.com",mailbox="foo@bar.com"} 3.221225472e+09
mailcow_mailbox_quota_allowed{host="mail.example.com",mailbox="test@bar.com"} 3.221225472e+09

# HELP mailcow_mailbox_quota_used Current syze of the mailbox in bytes
# TYPE mailcow_mailbox_quota_used gauge
mailcow_mailbox_quota_used{host="mail.example.com",mailbox="foo@bar.com"} 1.919023167e+09
mailcow_mailbox_quota_used{host="mail.example.com",mailbox="test@bar.com"} 1.844312552e+09

# HELP mailcow_mailq Length of the queue
# TYPE mailcow_mailq gauge
mailcow_mailq{host="mail.example.com",queue="deferred",sender="foo@bar.com"} 2
mailcow_mailq{host="mail.example.com",queue="deferred",sender="test@bar.com"} 1

# HELP mailcow_quarantine_age Age of quarantined items in seconds
# TYPE mailcow_quarantine_age histogram
mailcow_quarantine_age_bucket{host="mail.example.com",recipient="foo@bar.com",le="10800"} 0
mailcow_quarantine_age_bucket{host="mail.example.com",recipient="foo@bar.com",le="43200"} 0
mailcow_quarantine_age_bucket{host="mail.example.com",recipient="foo@bar.com",le="86400"} 0
mailcow_quarantine_age_bucket{host="mail.example.com",recipient="foo@bar.com",le="259200"} 3
mailcow_quarantine_age_bucket{host="mail.example.com",recipient="foo@bar.com",le="604800"} 9
mailcow_quarantine_age_bucket{host="mail.example.com",recipient="foo@bar.com",le="1.2096e+06"} 12
mailcow_quarantine_age_bucket{host="mail.example.com",recipient="foo@bar.com",le="2.592e+06"} 41
mailcow_quarantine_age_bucket{host="mail.example.com",recipient="foo@bar.com",le="+Inf"} 147
mailcow_quarantine_age_sum{host="mail.example.com",recipient="foo@bar.com"} 1.301292926e+09
mailcow_quarantine_age_count{host="mail.example.com",recipient="foo@bar.com"} 147
mailcow_quarantine_age_bucket{host="mail.example.com",recipient="test@bar.com",le="10800"} 0
mailcow_quarantine_age_bucket{host="mail.example.com",recipient="test@bar.com",le="43200"} 0
mailcow_quarantine_age_bucket{host="mail.example.com",recipient="test@bar.com",le="86400"} 0
mailcow_quarantine_age_bucket{host="mail.example.com",recipient="test@bar.com",le="259200"} 0
mailcow_quarantine_age_bucket{host="mail.example.com",recipient="test@bar.com",le="604800"} 0
mailcow_quarantine_age_bucket{host="mail.example.com",recipient="test@bar.com",le="1.2096e+06"} 0
mailcow_quarantine_age_bucket{host="mail.example.com",recipient="test@bar.com",le="2.592e+06"} 0
mailcow_quarantine_age_bucket{host="mail.example.com",recipient="test@bar.com",le="+Inf"} 2
mailcow_quarantine_age_sum{host="mail.example.com",recipient="test@bar.com"} 2.7138547e+07
mailcow_quarantine_age_count{host="mail.example.com",recipient="test@bar.com"} 2

# HELP mailcow_quarantine_score Score of quarantined mails according to rspamd
# TYPE mailcow_quarantine_score histogram
mailcow_quarantine_score_bucket{host="mail.example.com",recipient="foo@bar.com",le="0"} 0
mailcow_quarantine_score_bucket{host="mail.example.com",recipient="foo@bar.com",le="10"} 0
mailcow_quarantine_score_bucket{host="mail.example.com",recipient="foo@bar.com",le="20"} 41
mailcow_quarantine_score_bucket{host="mail.example.com",recipient="foo@bar.com",le="40"} 122
mailcow_quarantine_score_bucket{host="mail.example.com",recipient="foo@bar.com",le="60"} 136
mailcow_quarantine_score_bucket{host="mail.example.com",recipient="foo@bar.com",le="80"} 141
mailcow_quarantine_score_bucket{host="mail.example.com",recipient="foo@bar.com",le="100"} 141
mailcow_quarantine_score_bucket{host="mail.example.com",recipient="foo@bar.com",le="+Inf"} 147
mailcow_quarantine_score_sum{host="mail.example.com",recipient="foo@bar.com"} 16225.91000000001
mailcow_quarantine_score_count{host="mail.example.com",recipient="foo@bar.com"} 147
mailcow_quarantine_score_bucket{host="mail.example.com",recipient="test@bar.com",le="0"} 0
mailcow_quarantine_score_bucket{host="mail.example.com",recipient="test@bar.com",le="10"} 0
mailcow_quarantine_score_bucket{host="mail.example.com",recipient="test@bar.com",le="20"} 0
mailcow_quarantine_score_bucket{host="mail.example.com",recipient="test@bar.com",le="40"} 0
mailcow_quarantine_score_bucket{host="mail.example.com",recipient="test@bar.com",le="60"} 0
mailcow_quarantine_score_bucket{host="mail.example.com",recipient="test@bar.com",le="80"} 0
mailcow_quarantine_score_bucket{host="mail.example.com",recipient="test@bar.com",le="100"} 0
mailcow_quarantine_score_bucket{host="mail.example.com",recipient="test@bar.com",le="+Inf"} 2
mailcow_quarantine_score_sum{host="mail.example.com",recipient="test@bar.com"} 3988.03
mailcow_quarantine_score_count{host="mail.example.com",recipient="test@bar.com"} 2

# HELP mailcow_quarantine_count Number of mails currently in quarantine
# TYPE mailcow_quarantine_count gauge
mailcow_quarantine_count{host="mail.example.com",is_virus="0",recipient="foo@bar.com"} 147
mailcow_quarantine_count{host="mail.example.com",is_virus="0",recipient="test@bar.com"} 2
mailcow_quarantine_count{host="mail.example.com",is_virus="1",recipient="foo@bar.com"} 0
mailcow_quarantine_count{host="mail.example.com",is_virus="1",recipient="test@bar.com"} 0

# HELP mailcow_api_response_size Size of API response in bytes
# TYPE mailcow_api_response_size gauge
mailcow_api_response_size{host="mail.example.com",endpoint="api/v1/get/mailbox/all",statusCode="200"} 4395
mailcow_api_response_size{host="mail.example.com",endpoint="api/v1/get/mailq/all",statusCode="200"} 1798
mailcow_api_response_size{host="mail.example.com",endpoint="api/v1/get/quarantine/all",statusCode="200"} 47898

# HELP mailcow_api_response_time Response time of the API in milliseconds (1/1000s of a second)
# TYPE mailcow_api_response_time gauge
mailcow_api_response_time{host="mail.example.com",endpoint="api/v1/get/mailbox/all",statusCode="200"} 81
mailcow_api_response_time{host="mail.example.com",endpoint="api/v1/get/mailq/all",statusCode="200"} 615
mailcow_api_response_time{host="mail.example.com",endpoint="api/v1/get/quarantine/all",statusCode="200"} 46

# HELP mailcow_container_running 1 if the container is running, 0 if not
# TYPE mailcow_container_running gauge
mailcow_container_running{container="acme-mailcow",host="mail.example.com",image="mailcow/acme:1.74"} 1
mailcow_container_running{container="clamd-mailcow",host="mail.example.com",image="mailcow/clamd:1.36"} 1
mailcow_container_running{container="dockerapi-mailcow",host="mail.example.com",image="mailcow/dockerapi:1.37"} 1
mailcow_container_running{container="dovecot-mailcow",host="mail.example.com",image="mailcow/dovecot:1.130"} 1
mailcow_container_running{container="ipv6nat-mailcow",host="mail.example.com",image="robbertkl/ipv6nat"} 1
mailcow_container_running{container="memcached-mailcow",host="mail.example.com",image="memcached:alpine"} 1
mailcow_container_running{container="mysql-mailcow",host="mail.example.com",image="mariadb:10.4"} 1
mailcow_container_running{container="netfilter-mailcow",host="mail.example.com",image="mailcow/netfilter:1.38"} 1
mailcow_container_running{container="nginx-mailcow",host="mail.example.com",image="nginx:mainline-alpine"} 1
mailcow_container_running{container="olefy-mailcow",host="mail.example.com",image="mailcow/olefy:1.3"} 1
mailcow_container_running{container="php-fpm-mailcow",host="mail.example.com",image="mailcow/phpfpm:1.69"} 1
mailcow_container_running{container="postfix-exporter",host="mail.example.com",image="unikum/postfix_exporter"} 1
mailcow_container_running{container="postfix-mailcow",host="mail.example.com",image="mailcow/postfix:1.51"} 1
mailcow_container_running{container="redis-mailcow",host="mail.example.com",image="redis:5-alpine"} 1
mailcow_container_running{container="rspamd-mailcow",host="mail.example.com",image="mailcow/rspamd:1.68"} 1
mailcow_container_running{container="sogo-mailcow",host="mail.example.com",image="mailcow/sogo:1.83"} 1
mailcow_container_running{container="solr-mailcow",host="mail.example.com",image="mailcow/solr:1.7"} 1
mailcow_container_running{container="unbound-mailcow",host="mail.example.com",image="mailcow/unbound:1.12"} 1
mailcow_container_running{container="watchdog-mailcow",host="mail.example.com",image="mailcow/watchdog:1.82"} 1

# HELP mailcow_container_start Unix timestamp of the container start
# TYPE mailcow_container_start gauge
mailcow_container_start{container="acme-mailcow",host="mail.example.com",image="mailcow/acme:1.74"} 1.599247354e+09
mailcow_container_start{container="clamd-mailcow",host="mail.example.com",image="mailcow/clamd:1.36"} 1.599247354e+09
mailcow_container_start{container="dockerapi-mailcow",host="mail.example.com",image="mailcow/dockerapi:1.37"} 1.599247354e+09
mailcow_container_start{container="dovecot-mailcow",host="mail.example.com",image="mailcow/dovecot:1.130"} 1.599247354e+09
mailcow_container_start{container="ipv6nat-mailcow",host="mail.example.com",image="robbertkl/ipv6nat"} 1.599247354e+09
mailcow_container_start{container="memcached-mailcow",host="mail.example.com",image="memcached:alpine"} 1.599247354e+09
mailcow_container_start{container="mysql-mailcow",host="mail.example.com",image="mariadb:10.4"} 1.599247354e+09
mailcow_container_start{container="netfilter-mailcow",host="mail.example.com",image="mailcow/netfilter:1.38"} 1.599247354e+09
mailcow_container_start{container="nginx-mailcow",host="mail.example.com",image="nginx:mainline-alpine"} 1.599247354e+09
mailcow_container_start{container="olefy-mailcow",host="mail.example.com",image="mailcow/olefy:1.3"} 1.599247354e+09
mailcow_container_start{container="php-fpm-mailcow",host="mail.example.com",image="mailcow/phpfpm:1.69"} 1.599247354e+09
mailcow_container_start{container="postfix-exporter",host="mail.example.com",image="unikum/postfix_exporter"} 1.599247354e+09
mailcow_container_start{container="postfix-mailcow",host="mail.example.com",image="mailcow/postfix:1.51"} 1.599247354e+09
mailcow_container_start{container="redis-mailcow",host="mail.example.com",image="redis:5-alpine"} 1.599247354e+09
mailcow_container_start{container="rspamd-mailcow",host="mail.example.com",image="mailcow/rspamd:1.68"} 1.599247354e+09
mailcow_container_start{container="sogo-mailcow",host="mail.example.com",image="mailcow/sogo:1.83"} 1.599247354e+09
mailcow_container_start{container="solr-mailcow",host="mail.example.com",image="mailcow/solr:1.7"} 1.599247354e+09
mailcow_container_start{container="unbound-mailcow",host="mail.example.com",image="mailcow/unbound:1.12"} 1.599247354e+09
mailcow_container_start{container="watchdog-mailcow",host="mail.example.com",image="mailcow/watchdog:1.82"} 1.599247354e+09
```
