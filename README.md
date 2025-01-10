# CortexPlayground-AntropicClaude3-5
```sql
create or replace stage docs ENCRYPTION = (TYPE = 'SNOWFLAKE_SSE') DIRECTORY = ( ENABLE = true );

ls @docs;

-- Upload files to internal stage

create OR replace table transcript_table (call_text TEXT, category VARCHAR(16777216) );

INSERT INTO transcript_table (call_text, category)
SELECT to_varchar(
SNOWFLAKE.CORTEX.PARSE_DOCUMENT(
@docs,
'CORRECTED-TRANSCRIPT_-Snowflake-Inc-SNOW-US-Q3-2025-Earnings-Call-20-November-2024-5_00-PM-ET.pdf',{'mode' : 'LAYOUT'}
)), 'FY25-Q3'
;

INSERT INTO transcript_table (call_text, category)
SELECT to_varchar(
SNOWFLAKE.CORTEX.PARSE_DOCUMENT(
@docs,
'Snowflake-Q4-FY24-Earnings-Call-Transcript.pdf',{'mode' : 'LAYOUT'}
)), 'FY24-Q4'
;

select * from transcript_table;

```
