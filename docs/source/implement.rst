4. Development environment
===========================

The latest version of the SCVdb tool database operates on CentOS 7.7.1908.
We deploy and release the project to the remote server via a Dockerfile
within the `IntelliJ IDEA <https://www.jetbrains.com.cn/en-us/idea/>`_ 2021.3 integrated development environment.
The remote server has pre-installed `Docker <https://www.docker.com>`_ 19.03.5, and the backend
API along with the frontend pages are reverse-proxied through `Nginx <https://nginx.org/>`_ 1.22.0.
The project employs a technical architecture that separates the frontend
from the backend. On the backend side, business logic processing is built
upon the `Spring Boot <https://spring.io/projects/spring-boot>`_ 3.0.5 framework, which is based on Java 17.0.1.
For database management, `MyBatis-plus <https://github.com/baomidou/mybatis-plus>`_ 3.5.7 serves as the ORM framework,
connecting to a `MySQL <https://www.mysql.com/>`_ structured database set up through the Docker
container version mysql:8.0.32. To enhance system performance, we have
introduced the `Redis <https://redis.io/>`_ 6.2.11-alpine Docker container version as a caching
mechanism. The frontend is developed using the `Vue <https://vuejs.org/>`_ 3.2.4 framework within
a `Node.js <https://nodejs.org/en>`_ v16.13.0 environment. For frontend page development, `Axios <https://www.axiosdev.com.au>`_ 0.21.4
is used for data interaction with the backend API, while `Bootstrap <https://getbootstrap.com/>`_ v5.1.3
and `Element-UI <https://element-plus.org/en-US/>`_ (element-plus 2.2.0) provide page layout and style design
tools. `Font Awesome <https://fontawesome.com/>`_ 6.1.1 provides icon style support, and `Echarts <https://echarts.apache.org/en/index.html>`_ 5.3.1,
`Plotly <https://plotly.com/>`_ 2.23.0 and `CanvasXpress <https://canvasxpress.org/>`_ 38.4.1 are used for graph visualization.

To ensure the smoothest browsing experience, it is recommended that users
access the website with modern web browsers that support HTML5 standard,
such as **Firefox**, Google Chrome and Edge.












