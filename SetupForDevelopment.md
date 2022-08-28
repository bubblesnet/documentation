
install activemq version 
install postgres
git clone XXXX

In Postgres:
create databases named "bubbles", "bubbles_test" and "bubbles_dev"
cd controller/server/migrations
migrate_dev_up_all
migrate_test_up_all
migrate_prod_test_all

Create a user "admin" with manual hash
Create a site and station


