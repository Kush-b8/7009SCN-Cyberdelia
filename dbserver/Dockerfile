# CYBERDELIA database :: THE GIBSON

FROM postgres:11

# client's server config (drop it in the usual place)
COPY postgresql.conf /postgres/postgresql.conf
COPY pg_hba.conf     /postgres/pg_hba.conf

COPY sqlconfig/*.sql /docker-entrypoint-initdb.d/

EXPOSE 5432
