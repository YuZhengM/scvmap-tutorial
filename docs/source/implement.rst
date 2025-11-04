4. Development environment
===========================

 | scVMAP: https://bio.liclab.net/scvmap/
 | scVMAP front-end: https://github.com/YuZhengM/scvmap_web
 | scVMAP back-end: https://github.com/YuZhengM/scvmap
 | scVMAP API: https://bio.liclab.net/scvmap_service/scvmap.html

The current version of the scVMAP database is deployed on `CentOS <https://www.centos.org/>`_ 7.7.1908.
We deploy and release the front-end and back-end projects to the remote server separately using
Dockerfile in the integrated development environments
using `WebStorm <https://www.jetbrains.com.cn/en-us/webstorm/>`_ 2025.1.1.1
and `IntelliJ IDEA <https://www.jetbrains.com.cn/en-us/idea/>`_ 2025.1.1.1.
The remote server has pre-installed `Docker <https://www.docker.com>`_ 19.03.5, and the back-end API along with
the front-end pages are reverse-proxied through `Nginx <https://nginx.org/>`_ 1.22.0. The project employs a technical
architecture that separates the front-end from the back-end. On the backend side, business logic processing is built
upon the `Spring Boot <https://spring.io/projects/spring-boot>`_ 3.0.5 framework,
which is based on `Java <https://www.java.com/>`_ 17.0.1. For database management,
`MyBatis-plus <https://github.com/baomidou/mybatis-plus>`_ 3.5.7 serves as the ORM framework,
connecting to a `MySQL <https://www.mysql.com/>`_ structured database set up
through the Docker image mysql:8.0.32. To enhance system performance, we have
introduced the `Redis <https://redis.io/>`_ 6.2.11-alpine Docker container image as a caching mechanism.
The frontend is developed using the `Vue <https://vuejs.org/>`_ 3.2.4 framework within
a `Node.js <https://nodejs.org/en>`_ v16.13.0 environment.
For front-end page development, `Axios <https://www.axiosdev.com.au>`_ 0.21.4 is used
for data interaction with the backend API, while `Bootstrap <https://getbootstrap.com/>`_ v5.1.3
and `Element Plus <https://element-plus.org/en-US/>`_ (element-plus 2.2.0) provide page
layout and style design tools. `Font Awesome <https://fontawesome.com/>`_ 6.1.1 provides icon style support.
`Echarts <https://echarts.apache.org/en/index.html>`_ 5.3.1, `Plotly <https://plotly.com/>`_ 2.23.0 and
`CanvasXpress <https://canvasxpress.org/>`_ 38.4.1 are used for graph visualization. For the best experience,
we recommend using a modern, HTML5-compliant web browser such as Firefox, Google Chrome, or Edge.

To ensure the smoothest browsing experience, it is recommended that users
access the website with modern web browsers that support HTML5 standard,
such as **Firefox**, Google Chrome and Edge.
