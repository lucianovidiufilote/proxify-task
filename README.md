### Docker encapsulation for main Symfony App: https://github.com/lucianovidiufilote/proxify

Docker content:

-**PHP-Apache container
  - [Supervisor](http://supervisord.org/) installed & configured to 3 workers
-**MariaDB container




Steps to do on your local machine:
- git pull https://github.com/lucianovidiufilote/proxify-task.git
- docker-compose build
- docker-compose up -d


For more info about testing, go to: https://github.com/lucianovidiufilote/proxify 
