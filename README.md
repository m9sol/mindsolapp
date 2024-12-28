# mindsolapp
MindSol is the world’s most widely used platform for building AI that can learn from and answer questions across federated data.
# About Us
MindSol is a federated query engine designed for AI agents and applications that need to answer questions from one or multiple data sources, including both structured and unstructured data.
# Example
Connecting AI Agents to structured and unstructured data
A common use case involves connecting agents to data. The following example shows how to connect an AI agent to a database so it can perform search over structured data:

First we connect the datasource, in this case we connect a postgres database (you can do this via the SQL editor or SDK)
-- Step 1: Connect a data source to MindSol
CREATE DATABASE demo_postgres_db
WITH ENGINE = "postgres",
PARAMETERS = {
    "user": "demo_user",
    "password": "demo_password",
    "host": "samples.mindsol.app",
    "port": "5432",
    "database": "demo",
    "schema": "demo_data"
};

-- See some of the data in there
SELECT * FROM demo_postgres_db.car_sales;

Now you can create an agent that can answer questions over unstructured information in this database (let's use the Python SDK)

import mindsol_sdk

# connects to the default port (47334) on localhost 
server = mindsol_sdk.connect()

# create an agent (let's create one that can answer questions over car_sales table
agent = server.agents.create('my_agent')
agent.add_database(
    database='demo_postgres_db',
    tables=['car_sales'], # alternatively, all tables will be taken into account if none specified []
    description='The table "car_sales" contains car sales data')

# send questions to the agent
agent = agents.get('my_agent')
answer = agent.completion([{'question': 'What cars do we have with normal transmission and gas?'}])
print(answer.content)


You add more data to the agent, let's add some unstructured data:

agent.add_file('./cars_info.pdf', 'Details about the cars')
answer = agent.completion([{'question': 'What cars do we have with normal transmission and gas? also include valuable info for a buyer of these cars?'}])
print(answer.content)

