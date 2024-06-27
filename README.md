# novax-exchange-test

## Problem Try To Solve
1. File management system is system that used for file maintenance with different permission 
1. Support CRUD operations
    1. User can create new files/folder
    2. User can delete existing files/folders
    3. User can view the files/folder under current folder
2. User access control
    1. User able to login to get their identity
    2. User able to do their action according to file permission (Read/Write)

## API Required
| Endpoint | Description | Remark
| -------- | ------- | -------
| Create file/folder with permission  | With different input of permission(Can have default permission for better experience), user will be able to create file/folder under particular directory    |
| Delete file/folder | User will be able to delete file/folder under particular directory     | User can only delete their created files by passing directory id or file id
| Get the current files/folders under particular directory    | User can get the files and folders under particular directory    |
| Login | With username and password input, the API return the jwt token user for the above API|


## High Level Overview
1. 


## Database Schema 
1. File
```
CREATE TABLE file (
    id INT PRIMARY KEY,
    file_name VARCHAR(255) NOT NULL,
    file_path VARCHAR(255) NOT NULL,
    file_actual_path VARCHAR(255) NOT NULL COMMENT 'actual storage path'
    file_size BIGINT,
    creation_date DATETIME,
    modification_date DATETIME,
    owner_id INT,
    directory_id INT, 
    FOREIGN KEY (owner_id) REFERENCES user (id)
    FOREIGN KEY (file_directory_id) REFERENCES directory (id)
);
```
2. Directory
```
CREATE TABLE directory (
    id INT PRIMARY KEY,
    directory_name VARCHAR(255) NOT NULL,
    parent_directory_id INT,
    creation_date DATETIME,
    owner_id INT,
    FOREIGN KEY (owner_id) REFERENCES user(id)
);
```
3. User
```
CREATE TABLE user (
    id INT PRIMARY KEY,
    user_name VARCHAR(50) NOT NULL UNIQUE,
    password_hash VARCHAR(255) NOT NULL
);

```
4. Permission
```
-- For simplicity, the file created at the time must have permission id, don't have folder permission
CREATE TABLE permission (
    id INT PRIMARY KEY,
    file_id INT,
    permission_desc VARCHAR(255) NOT NULL COMMENT 'Describe the permission'
    FOREIGN KEY (file_id) REFERENCES file(id)
);
```
5. File Permission
```
-- This is one to n relationship
-- For simplicity, we have read/write permission for particular file
CREATE TABLE file_permission (
    id INT PRIMARY KEY,
    file_id INT,
    permission_id INT,
    FOREIGN KEY (file_id) REFERENCES file(id)
    FOREIGN KEY (permission_id) REFERENCES permission(id),
    UNIQUE KEY `composite_key` (`file_id`,`permission_id`)
);
```


## Readme for GUI

## Trade off / Potential Improvement
