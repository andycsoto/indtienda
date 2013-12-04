indtienda
=========

Crear un Virtual Environment que se encuentre en la carpeta del proyecto (paralelo al repo).
En este Virtual Environment se debe installar django 1.4 y oscar 0.6

Luego hay que hacer syncdb y migrate

Para lograr tirar esto a produccion en webfaction tuve que dejar el sgte httpd.conf

#httpd.conf
ServerRoot "/home/andycsoto/webapps/indtienda/apache2"

LoadModule dir_module        modules/mod_dir.so
LoadModule env_module        modules/mod_env.so
LoadModule log_config_module modules/mod_log_config.so
LoadModule mime_module       modules/mod_mime.so
LoadModule rewrite_module    modules/mod_rewrite.so
LoadModule setenvif_module   modules/mod_setenvif.so
LoadModule wsgi_module       modules/mod_wsgi.so

KeepAlive Off
Listen 25070
MaxSpareThreads 3
MinSpareThreads 1
ServerLimit 1
SetEnvIf X-Forwarded-SSL on HTTPS=1
ThreadsPerChild 5

WSGIRestrictEmbedded On
WSGILazyInitialization On

NameVirtualHost *
<VirtualHost *>
ServerName andycsoto.webfactional.com
LogFormat "%{X-Forwarded-For}i %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\"" combined
CustomLog /home/andycsoto/logs/user/access_indtienda.log combined
ErrorLog /home/andycsoto/logs/user/error_indtienda.log
WSGIDaemonProcess indtienda processes=2 threads=12 python-path=/home/andycsoto/webapps/indtienda/indtienda:/home/andycsoto/webapps/indtienda/oscar/lib/python2.7/site-packages:/home/andycsoto/webapps/test_app/lib/python2.7
WSGIProcessGroup indtienda
WSGIScriptAlias / /home/andycsoto/webapps/indtienda/indtienda/indtienda/wsgi.py

</VirtualHost>

WSGIPythonHome /home/andycsoto/webapps/indtienda/oscar

#Fin de httpd.conf
