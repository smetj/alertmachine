alertmachine
============

An alternative way to deal with Nagios/Naemon alerts.

Every time a host/service object changes state the *submit_alert_event*
command is triggered, which generates a rich JSON document containing
information about the host/service.  This JSON document is then submitted to
the *alertmachine* daemon either through UDP or Gearmand after which it it
evaluated and processed.

Installation
============

    $ pip install -U pyseps
    $ pip install -U wb_input_udp
    $ pip install -U wb_function_json
    $ pip install -U wb_function_template
    $ pip install -U wb_output_email


Nagios/Naemon configuration
===========================

Define 2 commands, one for host and one for service alerts:

    define command{
        command_name    alert_host
        command_line    /usr/bin/submit_alert_event --host_name $HOSTDISPLAYNAME$ --json '{"hostalias": "$HOSTALIAS$", "hostactionurl": "$HOSTACTIONURL$", "notificationauthorname": "$NOTIFICATIONAUTHORNAME$", "hosteventid": "$HOSTEVENTID$", "hostpercentchange": "$HOSTPERCENTCHANGE$", "timet": "$TIMET$", "notificationtype": "$NOTIFICATIONTYPE$", "lasthoststate": "$LASTHOSTSTATE$", "lasthoststateid": "$LASTHOSTSTATEID$", "hostnotificationnumber": "$HOSTNOTIFICATIONNUMBER$", "hostackcomment": "$HOSTACKCOMMENT$", "notificationrecipients": "$NOTIFICATIONRECIPIENTS$", "longhostoutput": "$LONGHOSTOUTPUT$", "hostdurationsec": "$HOSTDURATIONSEC$", "hostoutput": "$HOSTOUTPUT$", "hostgroupname": "$HOSTGROUPNAME$", "notificationauthor": "$NOTIFICATIONAUTHOR$", "notificationcomment": "$NOTIFICATIONCOMMENT$", "servicenotificationid": "$SERVICENOTIFICATIONID$", "totalhostservicescritical": "$TOTALHOSTSERVICESCRITICAL$", "hostname": "$HOSTNAME$", "totalhostservicesok": "$TOTALHOSTSERVICESOK$", "totalhostservicesunknown": "$TOTALHOSTSERVICESUNKNOWN$", "hoststate": "$HOSTSTATE$", "hostdowntime": "$HOSTDOWNTIME$", "hostdisplayname": "$HOSTDISPLAYNAME$", "totalhostserviceswarning": "$TOTALHOSTSERVICESWARNING$", "servicenotificationnumber": "$SERVICENOTIFICATIONNUMBER$", "nextvalidtime": "$NEXTVALIDTIME$", "hoststateid": "$HOSTSTATEID$", "longdatetime": "$LONGDATETIME$", "lasthostcheck": "$LASTHOSTCHECK$", "hostackauthoralias": "$HOSTACKAUTHORALIAS$", "lasthostunreachable": "$LASTHOSTUNREACHABLE$", "hoststatetype": "$HOSTSTATETYPE$", "hostgroupnames": "$HOSTGROUPNAMES$", "notificationisescalated": "$NOTIFICATIONISESCALATED$", "lasthostproblemid": "$LASTHOSTPROBLEMID$", "hostackauthorname": "$HOSTACKAUTHORNAME$", "hostnotes": "$HOSTNOTES$", "hostaddress": "$HOSTADDRESS$", "hostduration": "$HOSTDURATION$", "date": "$DATE$", "hostlatency": "$HOSTLATENCY$", "hostnotesurl": "$HOSTNOTESURL$", "maxhostattempts": "$MAXHOSTATTEMPTS$", "hostnotificationid": "$HOSTNOTIFICATIONID$", "hostexecutiontime": "$HOSTEXECUTIONTIME$", "totalhostservices": "$TOTALHOSTSERVICES$", "lasthostdown": "$LASTHOSTDOWN$", "hostcheckcommand": "$HOSTCHECKCOMMAND$", "shortdatetime": "$SHORTDATETIME$", "hostperfdata": "$HOSTPERFDATA$", "hostackauthor": "$HOSTACKAUTHOR$", "lasthosteventid": "$LASTHOSTEVENTID$", "hostproblemid": "$HOSTPROBLEMID$", "lasthoststatechange": "$LASTHOSTSTATECHANGE$", "notificationauthoralias": "$NOTIFICATIONAUTHORALIAS$", "time": "$TIME$", "isvalidtime": "$ISVALIDTIME$", "lasthostup": "$LASTHOSTUP$", "hostattempt": "$HOSTATTEMPT$", "umi_environment":"$USER3$", "object_type":"host"}'
    }

    define command{
        command_name    alert_service
        command_line    /usr/bin/submit_alert_event --host_name $HOSTDISPLAYNAME$ --service_name "$SERVICEDISPLAYNAME$" --json '{"servicenotificationnumber": "$SERVICENOTIFICATIONNUMBER$", "servicelatency": "$SERVICELATENCY$", "lasthoststate": "$LASTHOSTSTATE$", "servicecheckcommand": "$SERVICECHECKCOMMAND$", "notificationrecipients": "$NOTIFICATIONRECIPIENTS$", "longhostoutput": "$LONGHOSTOUTPUT$", "hostoutput": "$HOSTOUTPUT$", "servicedisplayname": "$SERVICEDISPLAYNAME$", "servicegroupname": "$SERVICEGROUPNAME$", "lastserviceunknown": "$LASTSERVICEUNKNOWN$", "servicenotificationid": "$SERVICENOTIFICATIONID$", "totalhostservices": "$TOTALHOSTSERVICES$", "longdatetime": "$LONGDATETIME$", "totalhostserviceswarning": "$TOTALHOSTSERVICESWARNING$", "lastservicestatechange": "$LASTSERVICESTATECHANGE$", "lasthostcheck": "$LASTHOSTCHECK$", "lasthostproblemid": "$LASTHOSTPROBLEMID$", "hostactionurl": "$HOSTACTIONURL$", "hostnotesurl": "$HOSTNOTESURL$", "maxserviceattempts": "$MAXSERVICEATTEMPTS$", "serviceackcomment": "$SERVICEACKCOMMENT$", "hostperfdata": "$HOSTPERFDATA$", "isvalidtime": "$ISVALIDTIME$", "lasthoststatechange": "$LASTHOSTSTATECHANGE$", "hostalias": "$HOSTALIAS$", "lasthosteventid": "$LASTHOSTEVENTID$", "nextvalidtime": "$NEXTVALIDTIME$", "lastservicewarning": "$LASTSERVICEWARNING$", "totalhostservicesunknown": "$TOTALHOSTSERVICESUNKNOWN$", "hoststate": "$HOSTSTATE$", "hoststateid": "$HOSTSTATEID$", "hostdisplayname": "$HOSTDISPLAYNAME$", "serviceproblemid": "$SERVICEPROBLEMID$", "serviceackauthorname": "$SERVICEACKAUTHORNAME$", "notificationauthor": "$NOTIFICATIONAUTHOR$", "hostnotes": "$HOSTNOTES$", "servicedurationsec": "$SERVICEDURATIONSEC$", "notificationcomment": "$NOTIFICATIONCOMMENT$", "lastservicecritical": "$LASTSERVICECRITICAL$", "timet": "$TIMET$", "notificationtype": "$NOTIFICATIONTYPE$", "hostproblemid": "$HOSTPROBLEMID$", "serviceisvolatile": "$SERVICEISVOLATILE$", "hostattempt": "$HOSTATTEMPT$", "lastservicecheck": "$LASTSERVICECHECK$", "hosteventid": "$HOSTEVENTID$", "lasthoststateid": "$LASTHOSTSTATEID$", "serviceeventid": "$SERVICEEVENTID$", "servicegroupnames": "$SERVICEGROUPNAMES$", "hostdurationsec": "$HOSTDURATIONSEC$", "hostgroupname": "$HOSTGROUPNAME$", "lastserviceproblemid": "$LASTSERVICEPROBLEMID$", "serviceduration": "$SERVICEDURATION$", "hostlatency": "$HOSTLATENCY$", "servicedowntime": "$SERVICEDOWNTIME$", "longserviceoutput": "$LONGSERVICEOUTPUT$", "totalhostservicesok": "$TOTALHOSTSERVICESOK$", "time": "$TIME$", "servicedesc": "$SERVICEDESC$", "serviceperfdata": "$SERVICEPERFDATA$", "serviceactionurl": "$SERVICEACTIONURL$", "lasthostunreachable": "$LASTHOSTUNREACHABLE$", "notificationauthorname": "$NOTIFICATIONAUTHORNAME$", "notificationisescalated": "$NOTIFICATIONISESCALATED$", "servicenotesurl": "$SERVICENOTESURL$", "hostduration": "$HOSTDURATION$", "hostnotificationid": "$HOSTNOTIFICATIONID$", "hostexecutiontime": "$HOSTEXECUTIONTIME$", "lasthostdown": "$LASTHOSTDOWN$", "shortdatetime": "$SHORTDATETIME$", "lastserviceeventid": "$LASTSERVICEEVENTID$", "notificationauthoralias": "$NOTIFICATIONAUTHORALIAS$", "lasthostup": "$LASTHOSTUP$", "servicestateid": "$SERVICESTATEID$", "serviceackauthoralias": "$SERVICEACKAUTHORALIAS$", "hostpercentchange": "$HOSTPERCENTCHANGE$", "hostcheckcommand": "$HOSTCHECKCOMMAND$", "hostnotificationnumber": "$HOSTNOTIFICATIONNUMBER$", "servicenotes": "$SERVICENOTES$", "servicestatetype": "$SERVICESTATETYPE$", "totalhostservicescritical": "$TOTALHOSTSERVICESCRITICAL$", "hostname": "$HOSTNAME$", "serviceackauthor": "$SERVICEACKAUTHOR$", "hoststatetype": "$HOSTSTATETYPE$", "hostdowntime": "$HOSTDOWNTIME$", "lastservicestateid": "$LASTSERVICESTATEID$", "lastservicestate": "$LASTSERVICESTATE$", "hostgroupnames": "$HOSTGROUPNAMES$", "hostaddress": "$HOSTADDRESS$", "date": "$DATE$", "servicepercentchange": "$SERVICEPERCENTCHANGE$", "maxhostattempts": "$MAXHOSTATTEMPTS$", "lastserviceok": "$LASTSERVICEOK$", "serviceattempt": "$SERVICEATTEMPT$", "servicestate": "$SERVICESTATE$", "serviceoutput": "$SERVICEOUTPUT$", "serviceexecutiontime": "$SERVICEEXECUTIONTIME$", "umi_environment":"$USER3$", "object_type":"service" }'
    }

Obviously you will have to adapt the *submit_alert_event* command parameters
to make it work in your environment.

Create a *contact* and define both commands as *host_notification_commands*
and *service_notification_commands* respectively.  Then assign this contact to
all host and service objects.

The document defined in the --json parameter is composed out of all possible
macro values (http://nagios.sourceforge.net/docs/3_0/macrolist.html) of a
host/service.  This document is then merged with the results of the livestatus
query resulting in a rich document we can evaluate.

Usage
=====

    $ wishbone start --config /etc/alertmachine.yaml



