FROM jupyterhub/k8s-singleuser-sample:0.9.0

USER root
WORKDIR /opt

RUN wget https://downloads.apache.org/spark/spark-3.2.4/spark-3.2.4-bin-hadoop3.2.tgz\
    && tar -xvzf spark-3.2.4-bin-hadoop3.2.tgz \
    && rm spark-3.2.4-bin-hadoop3.2.tgz \
    && rm -rf /opt/spark-3.2.4-bin-hadoop3.2/examples /opt/spark-3.2.4-bin-hadoop3.2/data \
    && apt-get -y remove wget \
    && apt-get -y autoremove \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /usr/local

RUN apt-get update  \
    && apt-get -y install default-jre  \
    && apt-get -y install openjdk-11-jre-headless  \
    && apt-get -y install openjdk-11-jdk \
    && apt-get -y remove --purge default-jre \
    && apt-get -y autoremove \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /home/jovyan

ENV SPARK_VERSION=3.2
ENV JAVA_VERSION=11.0
ENV SPARK_HOME=/opt/spark-3.2.4-bin-hadoop3.2
ENV JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
ENV PATH=/opt/conda/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/opt/spark-3.2.4-bin-hadoop3.2/bin
ENV PATH=/opt/conda/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/opt/spark-3.2.4-bin-hadoop3.2/bin:/usr/lib/jvm/java-11-openjdk-amd64/bin

USER jovyan

# Install nbgrader and enable the extensions
RUN python3 -m pip install git+https://github.com/zessta/nbgrader.git && \
    jupyter nbextension install --sys-prefix --py nbgrader   && \
    jupyter nbextension enable --sys-prefix --py nbgrader   && \
    jupyter serverextension enable --sys-prefix --py nbgrader  && \
    jupyter nbextension disable --sys-prefix course_list/main --section=tree && \
    jupyter serverextension disable --sys-prefix nbgrader.server_extensions.course_list && \
    jupyter nbextension disable --sys-prefix assignment_list/main --section=tree && \
    jupyter serverextension disable --sys-prefix nbgrader.server_extensions.assignment_list && \
    python3 -m pip install ngshare_exchange==0.5.1  && \
    python3 -m pip install pandas && \
    python3 -m pip install psycopg2-binary && \
    python3 -m pip install pathlib && \
    python3 -m pip install keras && \
    python3 -m pip install scikit-learn && \
    python3 -m pip install numpy && \
    python3 -m pip install matplotlib && \
    python3 -m pip install seaborn && \
    python3 -m pip install nbgitpuller && \   
    python3 -m pip install findspark && \
    python3 -m pip install statsmodels && \ 
    python3 -m pip install bokeh && \ 
    python3 -m pip install beautifulsoup4 && \ 
    python3 -m pip install mysql-connector-python && \ 
    python3 -m pip install tensorflow && \
    python3 -m pip install torch && \
    python -m pip install --upgrade pip && \
    pip install jupyter_contrib_nbextensions==0.5.1   

COPY docker-common-files/snippets.json /opt/conda/lib/python3.7/site-packages/jupyter_contrib_nbextensions/nbextensions/snippets/snippets.json

RUN jupyter contrib nbextension install --sys-prefix &&\
    jupyter nbextension enable snippets/main --sys-prefix &&\
    jupyter nbextensions_configurator disable --sys-prefix

# Configure nbgrader
COPY docker-common-files/nbgrader_config.py /etc/jupyter/nbgrader_config.py