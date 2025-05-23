FROM meltano/meltano:v3.7.3

WORKDIR /project

# Install any additional requirements
COPY ./requirements.txt .
RUN pip install -r requirements.txt

COPY . .
RUN meltano install

ENTRYPOINT ["meltano"]
