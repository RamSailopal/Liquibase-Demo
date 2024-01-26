## Liquibase Demo

### Provisioning the environment using Docker

First create a docker network to allow Liquibase and Postgres containers to communicate

    docker network create liquibase

#### Postgres

Run a postgres docker container that is attached to the liquibase network, can be reached via the hostname **postgres**, with the username **postgres** and the password **test**

    docker run --network liquibase -p 5432:5432 --name postgres -e POSTGRES_PASSWORD=test -d postgres

Using a postgres client such as [pgAdmin](https://www.pgadmin.org/), create a connection to the database. Host name/address will be **localhost** and the password will be **test**

Create a new database called **test**

### Liquibase update

Run an update command to create a table called **liquibase** using the file **Changelog/changelog.json**

    docker run --network liquibase --rm -v "$PWD/Changelog:/liquibase/changelog" liquibase/liquibase --url jdbc:postgresql://postgres:5432/test?currentSchema=public --username=postgres --password=test --changeLogFile=changelog.json update

    ####################################################
    ##   _     _             _ _                      ##
    ##  | |   (_)           (_) |                     ##
    ##  | |    _  __ _ _   _ _| |__   __ _ ___  ___   ##
    ##  | |   | |/ _` | | | | | '_ \ / _` / __|/ _ \  ##
    ##  | |___| | (_| | |_| | | |_) | (_| \__ \  __/  ##
    ##  \_____/_|\__, |\__,_|_|_.__/ \__,_|___/\___|  ##
    ##              | |                               ##
    ##              |_|                               ##
    ##                                                ##
    ##  Get documentation at docs.liquibase.com       ##
    ##  Get certified courses at learn.liquibase.com  ##
    ##                                                ##
    ####################################################
    Starting Liquibase at 15:47:09 (version 4.25.1 #690 built at 2023-12-18 16:29+0000)
    Liquibase Version: 4.25.1
    Liquibase Open Source 4.25.1 by Liquibase
    Running Changeset: changelog.json::newtable1::Ram

    UPDATE SUMMARY
    Run:                          1
    Previously run:               0
    Filtered out:                 0
    -------------------------------
    Total change sets:            1

    Liquibase: Update has been successful. Rows affected: 1
    Liquibase command 'update' was executed successfully.

Tag the latest change with **tag1**

    docker run --network liquibase --rm -v "$PWD/Changelog:/liquibase/changelog" liquibase/liquibase --url jdbc:postgresql://postgres:5432/test?currentSchema=public --username=postgres --password=test --changeLogFile=changelog.json tag --tag "tag1"

    ####################################################
    ##   _     _             _ _                      ##
    ##  | |   (_)           (_) |                     ##
    ##  | |    _  __ _ _   _ _| |__   __ _ ___  ___   ##
    ##  | |   | |/ _` | | | | | '_ \ / _` / __|/ _ \  ##
    ##  | |___| | (_| | |_| | | |_) | (_| \__ \  __/  ##
    ##  \_____/_|\__, |\__,_|_|_.__/ \__,_|___/\___|  ##
    ##              | |                               ##
    ##              |_|                               ##
    ##                                                ##
    ##  Get documentation at docs.liquibase.com       ##
    ##  Get certified courses at learn.liquibase.com  ##
    ##                                                ##
    ####################################################
    Starting Liquibase at 15:47:16 (version 4.25.1 #690 built at 2023-12-18 16:29+0000)
    Liquibase Version: 4.25.1
    Liquibase Open Source 4.25.1 by Liquibase
    Successfully tagged 'postgres@jdbc:postgresql://postgres:5432/test?currentSchema=public'
    Liquibase command 'tag' was executed successfully.

In your Postgres client, you should see the new liquibase table as well as another **databasechangelog** table containing a log of changes so far. The databasechangelog table should have a column **tag** with the tag just added.

Run another update to add an additional club column to the liquibase table using the file **Changelog/changelog1.json**:

    docker run --network liquibase --rm -v "$PWD/Changelog:/liquibase/changelog" liquibase/liquibase --url jdbc:postgresql://postgres:5432/test?currentSchema=public --username=postgres --password=test --changeLogFile=changelog1.json update

    ####################################################
    ##   _     _             _ _                      ##
    ##  | |   (_)           (_) |                     ##
    ##  | |    _  __ _ _   _ _| |__   __ _ ___  ___   ##
    ##  | |   | |/ _` | | | | | '_ \ / _` / __|/ _ \  ##
    ##  | |___| | (_| | |_| | | |_) | (_| \__ \  __/  ##
    ##  \_____/_|\__, |\__,_|_|_.__/ \__,_|___/\___|  ##
    ##              | |                               ##
    ##              |_|                               ##
    ##                                                ##
    ##  Get documentation at docs.liquibase.com       ##
    ##  Get certified courses at learn.liquibase.com  ##
    ##                                                ##
    ####################################################
    Starting Liquibase at 15:49:54 (version 4.25.1 #690 built at 2023-12-18 16:29+0000)
    Liquibase Version: 4.25.1
    Liquibase Open Source 4.25.1 by Liquibase
    Running Changeset: changelog1.json::newcolumn1::Ram

    UPDATE SUMMARY
    Run:                          1
    Previously run:               0
    Filtered out:                 0
    -------------------------------
    Total change sets:            1

    Liquibase: Update has been successful. Rows affected: 1
    Liquibase command 'update' was executed successfully.

Again, tag the changes, this time with **tag2**:

    docker run --network liquibase --rm -v "$PWD/Changelog:/liquibase/changelog" liquibase/liquibase --url jdbc:postgresql://postgres:5432/test?currentSchema=public --username=postgres --password=test --changeLogFile=changelog.json tag --tag "tag2"

    ####################################################
    ##   _     _             _ _                      ##
    ##  | |   (_)           (_) |                     ##
    ##  | |    _  __ _ _   _ _| |__   __ _ ___  ___   ##
    ##  | |   | |/ _` | | | | | '_ \ / _` / __|/ _ \  ##
    ##  | |___| | (_| | |_| | | |_) | (_| \__ \  __/  ##
    ##  \_____/_|\__, |\__,_|_|_.__/ \__,_|___/\___|  ##
    ##              | |                               ##
    ##              |_|                               ##
    ##                                                ##
    ##  Get documentation at docs.liquibase.com       ##
    ##  Get certified courses at learn.liquibase.com  ##
    ##                                                ##
    ####################################################
    Starting Liquibase at 15:51:29 (version 4.25.1 #690 built at 2023-12-18 16:29+0000)
    Liquibase Version: 4.25.1
    Liquibase Open Source 4.25.1 by Liquibase
    Successfully tagged 'postgres@jdbc:postgresql://postgres:5432/test?currentSchema=public'
    Liquibase command 'tag' was executed successfully.

Again, the changes to the liquibase table, with the additional column, should be visible in Postgres.

### Liquibase rollback

To "revert" the previous create column step, we need to rollback to the tag **BEFORE** the last and so:

    docker run --network liquibase --rm -v "$PWD/Changelog:/liquibase/changelog" liquibase/liquibase --url jdbc:postgresql://postgres:5432/test?currentSchema=public --username=postgres --password=test --changeLogFile=changelog1.json rollback --tag "tag1"

    ####################################################
    ##   _     _             _ _                      ##
    ##  | |   (_)           (_) |                     ##
    ##  | |    _  __ _ _   _ _| |__   __ _ ___  ___   ##
    ##  | |   | |/ _` | | | | | '_ \ / _` / __|/ _ \  ##
    ##  | |___| | (_| | |_| | | |_) | (_| \__ \  __/  ##
    ##  \_____/_|\__, |\__,_|_|_.__/ \__,_|___/\___|  ##
    ##              | |                               ##
    ##              |_|                               ##
    ##                                                ##
    ##  Get documentation at docs.liquibase.com       ##
    ##  Get certified courses at learn.liquibase.com  ##
    ##                                                ##
    ####################################################
    Starting Liquibase at 16:11:58 (version 4.25.1 #690 built at 2023-12-18 16:29+0000)
    Liquibase Version: 4.25.1
    Liquibase Open Source 4.25.1 by Liquibase
    Rolling Back Changeset: changelog1.json::newcolumn1::Ram
    Liquibase command 'rollback' was executed successfully.

One last check of the liquibase table should show it as no longer having the club column.

### Row inserts

Where as some sql update conventions have automatic rollback built into liquibase (as we have seen for table column creation), row inserts have no automatic roll back functionality. Rollback instruction have to therefore be specified in the change log files as can be seen within **Changelog/changelog2.json** and **Changelog/changelog3.json**

changelog2.json contains a liquibase insert change type and then a delete change type within roll back. These can be tested in much the same way as we have done previously using **update**, **tag** and **rollback** referencing changelog2.json

As an alternative to insert and delete change types, native sql can be placed in the file using the sql change type. This is seen in changelog3.json. 

### References

[Liquibase](https://docs.liquibase.com/home.html)