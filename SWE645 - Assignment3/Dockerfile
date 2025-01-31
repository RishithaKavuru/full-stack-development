FROM ubuntu
RUN apt-get update && apt-get install -y openjdk-17-jdk && apt-get install -y unzip
RUN mkdir database

#COPY newhw3.war /database
COPY 645HW3/target/SWE645hw3-0.0.1-SNAPSHOT.jar /database/newjar.jar
#RUN unzip database/newhw3.war

WORKDIR /database

CMD ["bash"]

CMD ["/usr/bin/java", "-jar", "newjar.jar"]


EXPOSE 8080
