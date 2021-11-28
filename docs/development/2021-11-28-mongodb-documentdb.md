---
layout: default
title: MongoDB to DocumentDB
description: "MongoDB Atlas 데이터를 DocumentDB AWS 로 이동하기"
parent: Development
nav_order: 1
---

# MongoDB to DocumentDB
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

# MongoDB Atlas 데이터를 DocumentDB AWS 로 이동하기

AWS 를 이용하여 전체 서비스를 제공하기 위하여 기존에 무료로 사용하고 있던 MongoDB Atlas 의 데이터를 AWS DocumentDB 로 옮기기 위한 과정이다. 주의할 점은 AWS DocumentDB 접속은 EC2 에서 가능하며 동일한 보안 그룹에 속해야 한다. 랩탑과 같은 로컬 PC 에서는 접속할 수 없다.

## mongodump

MongoDB Atlas 에 저장된 데이터를 로컬에 저장한다. bson과 json 파일을 확인할 수 있다.

  ```bash
  mongodump --uri "mongodb+srv://<username>:<password>@<Atlas cluster address>/<database>" --out <localpath>
  ```

## mongorestore

로컬 파일을 DocumentDB 로 업로드한다.

  ```bash
  mongorestore --ssl --host="<DocumentDB cluster address>:27017" --username=<username> --password=<password> --db <database name> --sslCAFile <CAFile path> <localpath>
  ```

sslCAFile 은 Amazon DocumentDB 인증기관 인증서이다.

  ```bash
  wget https://s3.amazonaws.com/rds-downloads/rds-combined-ca-bundle.pem
  ```

## 참고

[Dumping, Restoring, Importing, and Exporting Data - AWS Developer Guide](https://docs.aws.amazon.com/documentdb/latest/developerguide/backup_restore-dump_restore_import_export_data.html)
