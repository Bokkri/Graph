MATCH (n)
DETACH DELETE n;
LOAD CSV WITH HEADERS FROM 'file:///users.csv' AS line
with line
limit 1000
CREATE (n:Person {user_id: line.user_id, first_name: line.first_name,last_name: line.last_name,gender:line.gender,date_of_birth:line.date_of_birth,phone_number:line.phone_number,infected:line.infected,date_diagnosis: line.date_diagnosis}) 
 CREATE CONSTRAINT FOR (n:Person) REQUIRE n.user_id IS UNIQUE
RETURN count(n)

LOAD CSV WITH HEADERS FROM 'file:///contacts.csv' AS line
CREATE (a:Person {user_id:line.reporting_user})
with line
MATCH
  (a:Person),
  (n:person)
WHERE a.user_id = n.user_id  
with line
create (a)-[c:contact{contact_start:line.contact_start,contact_end:line.contact_end,duration:line.duee}]->(b)
return type(c)
