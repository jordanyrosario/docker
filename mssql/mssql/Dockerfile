FROM mcr.microsoft.com/mssql/server:2019-CU13-ubuntu-20.04

# Create a config directory
RUN mkdir -p /usr/config
WORKDIR /usr/config
USER root
# Bundle config source
COPY ./*  /usr/config/

RUN chown mssql -R  /opt/mssql /opt/mssql-tools 
RUN chown  mssql -R   /opt/microsoft /opt/mssql-extensibility
RUN chown  mssql -R   /opt/microsoft /opt/mssql-extensibility

RUN chmod +x /usr/config/entrypoint.sh
RUN chmod +x /usr/config/configure-db.sh
RUN chown mssql -R  /var/opt/mssql
RUN mkdir -p /var/opt/mssql/data
RUN chown mssql -R  /var/opt/mssql
USER mssql

ENTRYPOINT ["./entrypoint.sh"]

# Tail the setup logs to trap the process
CMD ["tail -f /dev/null"]

HEALTHCHECK --interval=15s CMD /opt/mssql-tools/bin/sqlcmd -U sa -P $SA_PASSWORD -Q "select 1" && grep -q "MSSQL CONFIG COMPLETE" ./config.log
