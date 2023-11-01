ARG DBT_TAG=1.6.5-bigquery
# this docker has ARM architecture
FROM xemuliam/dbt:${DBT_TAG}

# install dbt postgres adaptor 
RUN pip install dbt-postgres

# install postgres client
RUN apt update \
    && apt install -y postgresql-client git tree

# create least privileged user
RUN groupadd -g 501 user && \
    useradd -m -u 500 user -g user

USER user:user

# remove dbt entrypoint
ENTRYPOINT []