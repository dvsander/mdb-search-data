# Semantic picture search with MongoDB

## Introduction

Should be used together with [mdb-search repo](https://github.com/dvsander/mdb-search), but can be used as a processing backend standalone.

This will download each movie's poster image and generate embeddings with  `clip-ViT-B-32`. Those pictures and their embeddings are stored in MongoDB.

## Set-up environment

You need `python3` and `pip`.

    python3 --version
    python3 -m ensurepip --upgrade
    pip3 install -r requirements.txt

You need a `MongoDB Atlas` cluster. This can be a free cluster, created on [cloud.mongodb.com](https://www.mongodb.com/atlas/database). Ensure database access and network access allow you to make a connection to the database. Note free clusters have a size and performance limitation, feel free to run this on a small paid cluster with lots more data.

You need to set some local environment variables, this can be local `.env` file

    MDB_CONN=<YOUR MongoDB Atlas connection string>
    DB="sample_mflix"
    COLL="embedded_movies"

## Preparing the data

### Option 1: Load from a backup

Unrar the dump files from the project directory, run the `mongorestore` to restore the `sample_mflix and `embedded_movies collection. You will need the [MongoDB command line database tools](https://www.mongodb.com/try/download/database-tools) for this.

    mongorestore --uri="mongodb+srv://..."

### Option 2: Create the vector search embeddings yourself on any collection

Load the sample dataset from MongoDB Atlas. It's available with the `...` in the cluster view. When that's done, run the `util.py` utility.

    python util.py

Attention: It will download pictures for movies and for those pictures create the embeddings using the `clip-ViT-B-32` model. This might take a while. Could be optimized with multithreading locally and batch insert_manys. It's normal that you run out of disk space around movie 3000 on the free tier, you can ignore the message.
