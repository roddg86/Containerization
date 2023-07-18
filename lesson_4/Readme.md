## Родионов Дмитрий | 3252 | Урок 4

#### Запуск файла java в контейнере

#### Текст обьяснения:

Создадим тестовый файл java с содержимым

>vim Sample.java

```java
class Sample{
     public static void main(String args[]){
         System.out.println("Welcome to GeekBrains");
     }
}
```

Создадим докерфайл следующего вида.  
В этом файле Dockerfile мы извлекли базовый образ Java 11
из DockerHub.  
Мы установили рабочий каталог и скопировали файлы в рабочий каталог.  
После этого мы скомпилировали наше Java-приложение
и запустили исполняемый файл.

>vim Dockerfile

>FROM adoptopenjdk/openjdk11:ubi  
WORKDIR /var/www/java  
COPY . /var/www/java  
RUN javac Sample.java  
CMD ["java", "Sample"]  

Создадим образ докера

>sudo docker build -t java-demo .

Запустим этот образ

>sudo docker run -it java-demo

#### Логи:

root@dmitry-vb:/home/roddg/doktest# vim Sample.java  
root@dmitry-vb:/home/roddg/doktest# vim Dockerfile  
root@dmitry-vb:/home/roddg/doktest# sudo docker build -t java-demo .  
Sending build context to Docker daemon  3.072kB  
Step 1/5 : FROM adoptopenjdk/openjdk11:ubi  
---> 900273569b80  
Step 2/5 : WORKDIR /var/www/java  
---> Using cache  
---> 7098da1e5914  
Step 3/5 : COPY . /var/www/java  
---> Using cache  
---> 6fd7ee68ccc2  
Step 4/5 : RUN javac Sample.java  
---> Using cache  
---> 36602f0d6b5c  
Step 5/5 : CMD ["java", "Sample"]  
---> Using cache  
---> f6693c5e9837  
Successfully built f6693c5e9837  
Successfully tagged java-demo:latest  
root@dmitry-vb:/home/roddg/doktest# sudo docker run -it java-demo  
Welcome to GeekBrains  
root@dmitry-vb:/home/roddg/doktest#  

#### История(history):

506  docker rm $(docker ps -a -q) --force  
507  docker ps -a  
508  vim Sample.java  
509  vim Dockerfile  
510  sudo docker build -t java-demo .  
511  sudo docker run -it java-demo  
512  history  

#### Скриншот:

![javaDocker.jpg](img%2FjavaDocker.jpg)