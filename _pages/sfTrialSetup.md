---
layout: default
title: "Setting Up Your Snowflake Environment for Notebooks"
permalink: /sfenvsetup-notebooks/
---
# Setting Up Your Snowflake Environment for Notebooks

Great approach wanting to document your setup steps in a SQL worksheet for reproducibility. Let's create a proper environment following Snowflake best practices.

## Initial Setup SQL Worksheet

First, let's create a SQL worksheet for your setup:

1. Navigate to Worksheets in the Snowflake UI
2. Create a new worksheet named "Notebook Environment Setup"
3. Add a comment header to document the purpose:

```sql
/*
SNOWFLAKE NOTEBOOK ENVIRONMENT SETUP
Created: [Current Date]
Purpose: Setup environment for working with Snowflake Notebooks
         including required database, schema, warehouse, and roles
*/
```

## Creating Proper Role Separation

You're absolutely right not to do everything as ACCOUNTADMIN. Let's create a dedicated role structure:

```sql
-- Create a dedicated role for notebook development
USE ROLE ACCOUNTADMIN;

-- Create a custom role for notebook work
CREATE ROLE IF NOT EXISTS NOTEBOOK_ADMIN;

-- Grant privileges to the role
GRANT CREATE DATABASE ON ACCOUNT TO ROLE NOTEBOOK_ADMIN;
GRANT CREATE WAREHOUSE ON ACCOUNT TO ROLE NOTEBOOK_ADMIN;

-- Create a development role with fewer privileges
CREATE ROLE IF NOT EXISTS NOTEBOOK_USER;

-- Assign roles to your user
GRANT ROLE NOTEBOOK_ADMIN TO USER LEEBASE;
GRANT ROLE NOTEBOOK_USER TO USER LEEBASE;

-- Switch to the notebook admin role for further setup
USE ROLE NOTEBOOK_ADMIN;
```

## Create Compute Resources

Let's create a dedicated warehouse for your notebook work:

```sql
-- Create a warehouse for notebook development
CREATE WAREHOUSE IF NOT EXISTS NOTEBOOK_WH 
WITH WAREHOUSE_SIZE = 'X-SMALL'
     AUTO_SUSPEND = 300
     AUTO_RESUME = TRUE
     INITIALLY_SUSPENDED = TRUE
     COMMENT = 'Warehouse for Snowflake notebook development';

-- Grant usage to both roles
GRANT USAGE ON WAREHOUSE NOTEBOOK_WH TO ROLE NOTEBOOK_USER;
GRANT USAGE ON WAREHOUSE NOTEBOOK_WH TO ROLE NOTEBOOK_ADMIN;

-- Activate the warehouse
USE WAREHOUSE NOTEBOOK_WH;
```

## Create Database and Schema

Now let's create the database structure needed for notebooks:

```sql
-- Create a database for notebooks
CREATE DATABASE IF NOT EXISTS NOTEBOOK_DB
COMMENT = 'Database for Snowflake notebook development and examples';

-- Create schemas for different purposes
CREATE SCHEMA IF NOT EXISTS NOTEBOOK_DB.EXAMPLES
COMMENT = 'Schema for example notebooks';

CREATE SCHEMA IF NOT EXISTS NOTEBOOK_DB.DEVELOPMENT
COMMENT = 'Schema for notebook development work';

-- Grant privileges to the user role
GRANT USAGE ON DATABASE NOTEBOOK_DB TO ROLE NOTEBOOK_USER;
GRANT USAGE ON SCHEMA NOTEBOOK_DB.EXAMPLES TO ROLE NOTEBOOK_USER;
GRANT USAGE ON SCHEMA NOTEBOOK_DB.DEVELOPMENT TO ROLE NOTEBOOK_USER;
GRANT CREATE TABLE, CREATE VIEW, CREATE STAGE, CREATE FUNCTION, CREATE PROCEDURE 
  ON SCHEMA NOTEBOOK_DB.DEVELOPMENT TO ROLE NOTEBOOK_USER;
GRANT SELECT, INSERT, UPDATE, DELETE ON ALL TABLES IN SCHEMA NOTEBOOK_DB.EXAMPLES TO ROLE NOTEBOOK_USER;
GRANT SELECT, INSERT, UPDATE, DELETE ON ALL TABLES IN SCHEMA NOTEBOOK_DB.DEVELOPMENT TO ROLE NOTEBOOK_USER;
GRANT SELECT ON ALL VIEWS IN SCHEMA NOTEBOOK_DB.EXAMPLES TO ROLE NOTEBOOK_USER;

-- Set context for further operations
USE DATABASE NOTEBOOK_DB;
USE SCHEMA DEVELOPMENT;
```

## Final Setup for Notebooks

```sql
-- Create a stage for notebook file uploads if needed
CREATE STAGE IF NOT EXISTS NOTEBOOK_DB.DEVELOPMENT.NOTEBOOK_STAGE
COMMENT = 'Stage for notebook-related files';

GRANT READ, WRITE ON STAGE NOTEBOOK_DB.DEVELOPMENT.NOTEBOOK_STAGE TO ROLE NOTEBOOK_USER;

-- Switch to the user role for regular notebook work
USE ROLE NOTEBOOK_USER;
USE WAREHOUSE NOTEBOOK_WH;
USE DATABASE NOTEBOOK_DB;
USE SCHEMA DEVELOPMENT;
```

## Uploading Your First Notebook

Now you're ready to upload the "My First Notebook.ipynb":

1. In the Snowflake UI, navigate to Notebooks
2. Click "Create" or "Import"
3. Select your .ipynb file
4. For the database, select NOTEBOOK_DB
5. For the schema, select DEVELOPMENT (or EXAMPLES if you prefer)

## Best Practices Summary

1. **Role separation**: NOTEBOOK_ADMIN for setup, NOTEBOOK_USER for daily work
2. **Resource isolation**: Dedicated warehouse with auto-suspend
3. **Organized database structure**: Separate schemas for examples vs. your work
4. **Documented setup**: SQL worksheet captures all steps for reproducibility
5. **Least privilege**: Only necessary grants to each role

This approach gives you a clean, organized environment that follows security best practices while making it easy to recreate in future trial accounts.

# Prompt that created this Claude 3.7 output
Play the role of advanced snowflake architect and technical educator.  I'm a snowflake architect.  I have just fired up a new trial account.  I am working through learning the snowflake notebooks feature.  I have downloaded the repository of example notebooks.  I want to start with the My First Notebook .ipynb - to upload it, I first need a database.  I'm logged in with use leebase as account admin.  I want to start a sql worksheet where I do all my setup steps so I can recreate the environment for the next trial if needed.  Help me do so with best practices.  I don't think I should do everything as account admin