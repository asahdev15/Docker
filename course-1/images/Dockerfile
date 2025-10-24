FROM ubuntu:latest
LABEL authors="asahdev"

RUN apt-get update && apt-get install -y python python-pip
RUN pip install flask flask-mysql

COPY . /opt/source-code

ENTRYPOINT ["sh", "-c", "FLASK_APP=/opt/source-code/app.py flask run"]

#######################################################################################################################

FROM ubuntu

# Update package lists and install Python and pip
RUN apt-get update && apt-get install -y python python-pip
# Install Flask via pip
RUN pip install flask

# Copy application source code into the container
COPY app.py /opt/app.py

# Set the entrypoint to run the Flask application
ENTRYPOINT ["flask", "run", "--host=0.0.0.0", "--app", "/opt/app.py"]



