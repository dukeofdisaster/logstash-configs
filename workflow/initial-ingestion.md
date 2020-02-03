# Initial Ingestion
I purposely left out basic ingestion stuff like beats,inputs, etc. Getting
the data to elasticsearch is the easy part, but getting it in the right place
with the right form with the right type is a different story.

## High level overview
Goal: Get logs into elasticsearch such that this data is meaningful, properly
typed, user-friendly, and efficient.

To me, the following steps are the easiest way of accomplishing this.

1. If possible, get to logs to logstash in standard json.
2. Output to elasticsearch
3. Standardize your fields with elasticsearch common schema
- [ ECS Field reference ](https://elastic.co/guide/en/current/ecs-field-reference.html)
- Fields now standardized, with default types; the result of elasticsearch
dynamic mapping.
4. Once index fields have been standardized, create a template for this index with appropriate datatypes.
- main consideration here is IP data type, "1.1.1.1" gets interpreted as a
a string, which has implications for users trying to do IP range searches or
anything with CIDR notation, which becomes esp. important when log data is used
for security purposes.
5. Lather rinse repeat

NOTE: (4) may not be necessary if we can use geoip in logstash filters, but I 
have not completely gone through this process myself, so can't say at this 
moment. Seems reasonable though as the default logstash template in templates/ 
has a geoip.ip field with the right type
