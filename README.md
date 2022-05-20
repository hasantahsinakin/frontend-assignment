# frontend-assignment
Docker backend for the BY front end assignment

[hasan@dotnet-deploy ~]$ cd /home/hasan/hasan_git_repo/frontend-assignment/
[hasan@dotnet-deploy frontend-assignment]$ clear
[hasan@dotnet-deploy frontend-assignment]$ docker-compose up
Starting frontend-assignment_database_1 ... done
Starting frontend-assignment_web_1      ... done
Attaching to frontend-assignment_database_1, frontend-assignment_web_1
database_1  |
database_1  | PostgreSQL Database directory appears to contain a database; Skipping initialization
database_1  |
database_1  | LOG:  database system was interrupted; last known up at 2022-05-20 13:32:27 UTC
database_1  | LOG:  database system was not properly shut down; automatic recovery in progress
database_1  | LOG:  invalid record length at 0/1527A28: wanted 24, got 0
database_1  | LOG:  redo is not required
database_1  | LOG:  MultiXact member wraparound protections are now enabled
database_1  | LOG:  autovacuum launcher started
database_1  | LOG:  database system is ready to accept connections
web_1       | => Booting Puma
web_1       | => Rails 5.0.0.1 application starting in development on http://0.0.0.0:3000
web_1       | => Run `rails server -h` for more startup options
web_1       | Puma starting in single mode...
web_1       | * Version 3.11.0 (ruby 2.3.3-p222), codename: Love Song
web_1       | * Min threads: 5, max threads: 5
web_1       | * Environment: development
web_1       | * Listening on tcp://0.0.0.0:3000
web_1       | Use Ctrl-C to stop
web_1       | Started GET "/" for 172.19.0.1 at 2022-05-20 18:51:34 +0000
web_1       | The PGconn, PGresult, and PGError constants are deprecated, and will be
web_1       | removed as of version 1.0.
web_1       |
web_1       | You should use PG::Connection, PG::Result, and PG::Error instead, respectively.
web_1       |
web_1       | Called from /usr/local/bundle/gems/activesupport-5.0.0.1/lib/active_support/dependencies.rb:259:in `load_dependency'
web_1       |   ActiveRecord::SchemaMigration Load (0.8ms)  SELECT "schema_migrations".* FROM "schema_migrations"
web_1       | Processing by Rails::WelcomeController#index as */*
web_1       |   Parameters: {"internal"=>true}
web_1       |   Rendering /usr/local/bundle/gems/railties-5.0.0.1/lib/rails/templates/rails/welcome/index.html.erb
web_1       |   Rendered /usr/local/bundle/gems/railties-5.0.0.1/lib/rails/templates/rails/welcome/index.html.erb (4.6ms)
web_1       | Completed 200 OK in 17ms (Views: 11.3ms | ActiveRecord: 0.0ms)
web_1       |
web_1       |

[hasan@dotnet-deploy ~]$ docker ps
CONTAINER ID   IMAGE                                        COMMAND                  CREATED        STATUS          PORTS                                                                                                                                  NAMES
3454c4620914   frontend-assignment_web                      "/bin/sh -c 'rm -f /…"   8 hours ago    Up 21 minutes   0.0.0.0:3000->3000/tcp, :::3000->3000/tcp                                                                                              frontend-assignment_web_1
1fe09e6e6118   postgres:9.6                                 "docker-entrypoint.s…"   11 hours ago   Up 21 minutes   0.0.0.0:5432->5432/tcp, :::5432->5432/tcp                                                                                              frontend-assignment_database_1
[hasan@dotnet-deploy ~]$ docker images
REPOSITORY                                     TAG           IMAGE ID       CREATED        SIZE
hasantahsinakin/frontend-assignment_web        v1            a2ff06d196c8   7 hours ago    872MB
frontend-assignment_web                        latest        46171bda48d2   8 hours ago    872MB
hasantahsinakin/frontend-assignment_database   v1            027ccf656dc1   3 months ago   200MB
postgres                                       9.6           027ccf656dc1   3 months ago   200MB
ruby                                           2.3.3         0e1db669d557   5 years ago    734MB


[hasan@dotnet-deploy ~]$ minikube tunnel
[sudo] password for hasan:

[hasan@dotnet-deploy ~]$ minikube kubectl -- create deployment frontend-assignment-database --image=hasantahsinakin/frontend-assignment_database:v1
[hasan@dotnet-deploy ~]$ minikube kubectl -- expose deployment frontend-assignment-database --type=LoadBalancer --port=5432

[hasan@dotnet-deploy ~]$ minikube kubectl -- create deployment frontend-assignment-web --image=hasantahsinakin/frontend-assignment_web:v1
[hasan@dotnet-deploy ~]$ minikube kubectl -- expose deployment frontend-assignment-web --type=LoadBalancer --port=3000

[hasan@dotnet-deploy ~]$ minikube kubectl get svc
NAME                           TYPE           CLUSTER-IP       EXTERNAL-IP      PORT(S)          AGE
frontend-assignment-database   LoadBalancer   10.101.181.112   10.101.181.112   5432:30099/TCP   21m
frontend-assignment-web        LoadBalancer   10.110.114.86    10.110.114.86    3000:30295/TCP   19m
kubernetes                     ClusterIP      10.96.0.1        <none>           443/TCP          5h15m

[hasan@dotnet-deploy ~]$ minikube service frontend-assignment-database --url
http://192.168.49.2:30099

[hasan@dotnet-deploy ~]$ minikube service frontend-assignment-web --url
http://192.168.49.2:30295

