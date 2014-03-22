submit_alert_event
==================

Create a rich JSON documents from Nagios events which can be used to evaluate
alert rules against.

Dependencies
============

    - Gearman::Client
    - Monitoring::Livestatus
    - JSON
    - UUID::Tiny

Usage
=====

    $ /usr/bin/submit_alert_event --help

    A script which creates JSON formatted events out of Nagios/Naemon alerts which
    then can be further processed by an external process.

    A Livestatus query is done for the host/service to collect all available data
    of the host/service.  To each event a UUID4 value (field uuid) is added as
    well as the alert type (field alert_type) to differentiate between a host or
    service alert.

    Usage: submit_alert_event.pl --host_name hostname [--service_name servicename]
    [--livestatus path|host:port] [--destination data] [--queue name] [--json
    data]

        --host_name     The Nagios/Naemon hostname to query.

    Options:

        --service_name  The Nagios/Naemon servicename to query.

        --livestatus    The livestatus socket to connect to.
                        When value contains : it is concidered a network location.
                        Default: "/var/lib/livestatus/livestatus.sock"

        --destination   The destination to submit the alert event.  Can be either
                        Gearmand or a UDP socket.
                        Default: gearmand://localhost:4730

        --queue         The Gearman queue to submit the alert event to if
                        --destination points to Gearmand.
                        Default: "alert_event"

        --json          A JSON document to merge with the Livestatus response.
